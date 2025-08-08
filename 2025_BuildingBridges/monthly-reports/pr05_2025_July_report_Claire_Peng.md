# Monthly Report - July: Incremental Typescript Migration for the p5.js Web Editor

## Overview

The p5.js web editor is a legacy codebase running on Node 16, with over 100,000 lines of code. Given the limited time available for this grant, my goal was not to migrate the entire client or server folder at once, but to **prepare both parts of the codebase and remove friction so that future contributors could treat TypeScript migration tasks as approachable, “good first issues.”**

To that end, I focused on configuring all necessary TypeScript tooling for both the server and client, and migrating as many files as possible within the rest of timeframe to serve as examples.

In July, I completed the initial setup of the client folder for TypeScript migration, including installing dependencies and configuring the tooling. I also migrated instances of both non-React files and React components to validate that the setup worked across different types of client-side files.

## Key Decisions and Progress:

- Setting up a root `tsconfig.json` that references individual configurations for both the client and server.
  - This allowed for incremental migration of the client and server separately.
- Setting up a root `tsconfig.base.json` that the `client/tsconfig.json` and `server/tsconfig.json` can extend on.
- Installed all Typescript dependencies.
- Setting up a typecheck script to run as a pre-commit hook.
- Configured TypeScript to automatically resolve imports without requiring explicit `.ts` or `.js` extensions.
- Migrated an instance of a non-React file (`client/utils/generateRandomName`)
- Migrated an instance of a React file (`client/common/SkipLink`)
- Migrating the entire `client/utils` folder
- Migrating the entire `client/common` folder

## Challenges and Solutions:

The primary challenge was setting up the correct versions of dependencies that were compatible with Node 16. Many TypeScript-related packages required manual version pinning, as newer versions often conflicted with the legacy environment.

Another challenge involved Prettier and ESLint integration. I encountered a bug where ESLint would indicate that Prettier flagged valid TypeScript syntax, such as the `as` keyword or angle brackets (`<T>`), as errors. I had already configured the ESLint setup to use the Babel parser for `.js` and `.jsx` files, and the ESLint TypeScript parser for `.ts` and `.tsx` files, so it was strange that this was happening.

At first, I thought this may be due to us having older version of Prettier or ESLint, which may have come before these pieces of Typescript syntax were introduced. I then spent time trying to upgrade them and even experimented with bumping Node from 16 to 18, but this caused widespread peer dependency conflicts, so I reverted the changes.

Eventually, I realized that the issue stemmed from Prettier being hardcoded to always use the Babel parser, which does not support TypeScript syntax, instead of different parsers depending on file-type as specified in the ESLint config. [Once I removed that hard-coded setting](https://github.com/processing/p5.js-web-editor/pull/3565/files#diff-663ade211b3a1552162de21c4031fcd16be99407aae5ceecbb491a2efc43d5d2L8), the TypeScript-related formatting errors disappeared.

Another challenge was that in my initial migration work, I forgot to use `git mv someFile.js someFile.ts` when changing the file extension, which broke history tracking and required remaking the branch to preserve it. I ended up finding that this flow of copying over changes to a final branch quite useful for my working style. I could work out the tricky aspects of migration freely in a messy working branch, then create a final branch with a clean, legible and linear commit history that future open-source contributors would find easier to read.

As for the actual migration process, it was mostly straightforward. I reviewed where each file or component was used, inferred the types of inputs and expected outputs, and added type annotations accordingly. In some cases, such as with the `getConfig()` function, I discovered issues with how edge cases were handled. That function retrieves environment variable values, but it sometimes returned `undefined`, which would be coerced to a `string` when used in template literals. Additionally, it sometimes returned `string` values when `booleans` or `numbers` were expected, which required explicit parsing, so I added a type-parsing utility function that can be used in conjunction with `getConfig()` in this instance. **For more details, please see PR #3 linked below.**

I also found a few functions that were no longer being used anywhere and was able to remove them. Others were refactored for better readability. Whenever I made changes like these, I added unit tests prior to secure against regression — especially for code that was significantly altered during migration.

## PR's:

- [pr05 Typescript #1: Set up TS dependencies (#3533)](https://github.com/processing/p5.js-web-editor/pull/3533)
  - Install ts dependencies for Node 16 and React 16
  - Set up tsconfigs & add typecheck on pre-commit hook
- [pr05 Typescript #2: automatically resolve imports of ts files & migrate instance of client/utils file (#3540)](https://github.com/processing/p5.js-web-editor/pull/3540)
  - Auto-resolve imports of ts files
  - Migrate `client/utils/generateRandomName()`
- [pr05 Typescript Migration 3: Migrate the client/utils folder (#3553)](https://github.com/processing/p5.js-web-editor/pull/3553)
  - Migrate entire client folder, with exception of CodeMirror & intellisense modules due to unfamiliarity
- [pr05 Typescript #4: migrate instance of react file (#3554)](https://github.com/processing/p5.js-web-editor/pull/3554)
  - Install missing ts react dependencies
- [pr05 Typescript Migration #5: Migrate client/common folder (#3565)](https://github.com/processing/p5.js-web-editor/pull/3565)
  - Fix issue with hard-coded `parser: babel` option in `.prettierrc`
  - Migrate entire `client/common` folder

## Next Steps:

In August, I plan to continue migrating as much of the client folder as possible. My top priority is to migrate a Redux reducer as an example, since that area of the codebase is less familiar to me. I’ll start by studying existing reducer implementations and proceed from there.
As well in August, I will start drafting documentation on how to migrate React files & how to write tests.
