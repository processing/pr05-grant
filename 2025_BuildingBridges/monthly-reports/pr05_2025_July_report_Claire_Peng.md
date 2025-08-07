# Monthly Report - July: Incremental Typescript Migration for the p5.js Web Editor

## Overview

The p5.js web editor is a legacy codebase running on Node 16, with over 100,000 lines of code. Given the limited time available for this grant, my goal was to prepare both the client and server portions of the codebase for incremental TypeScript migration. The aim was to remove friction so that future contributors could treat TypeScript migration tasks as approachable, “good first issues.”

To that end, I focused on configuring all necessary TypeScript tooling for both the server and client, and migrating a few files to serve as examples. In July, I completed the initial setup of the client folder for TypeScript migration, including installing dependencies and configuring the tooling. I also migrated instances of both non-React files and React components to validate that the setup worked across different types of client-side files.

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

- Main challenge was setting up the right dependencies
  - Had to look up versions of packages needed for typescript that were compatible with node 16 and specify the version each time
- Had a weird bug where eslint would indicate a prettier error for typescript key words/characters such as `as` or `<` (eg. `useState<boolean>(false)`) where prettier was not parsing the files as typescript
- I thought this was a version error -- perhaps `as` and `<` were later bits of syntax from the version of prettier on the app, or version of eslint on the app, or version of babel
- I spent a long time trying to upgrade these version, and even trying to bump the node version from 16 to 18, but this resulted in a lot of peer dependencies conflicts, so I dropped it
- I thought we can configured the eslint such that if it is a js, jsx file, use babel parser and if it's typescript, use the eslint typescript parser
- Then it turned out that prettier was hardcoded to always use the babel parser that can only read js, so it was flagging ts syntax as errors. The errors disappeared after I removed these

- As for the actual migration work, it was relatively straight forward. I had to find all instances of a file or component being used, and see what types we expect to plug into it and what types we expect out of it.
- There were occassions where the existing code wasn't handling edgecases well, such as getConfig()
  - function that returns the value of a key if the key exists in an env file
  - sometimes returns undefined, but it's being put in template literals, so it compiles to 'undefined'
  - sometimes expects a boolean or a number, but it's not being parsed into the correct type, and it a number-ish string or boolean-ish string
- There were other functions that once I tried to find them, weren't being used at all, so I was able to remove them
- There are some functions that got refactored for readability.
- I added unit test to as many migrated pieces of code as possible, especially if they were refactored in the process.

## PR's:

## Next Steps:

- August:
  - Migrate as many parts of the client folder as possible, prioritising to migrate an instance of a react redux reducer
  - I have the least experience with this bit of client-side code so I will need to study examples of reducer code
