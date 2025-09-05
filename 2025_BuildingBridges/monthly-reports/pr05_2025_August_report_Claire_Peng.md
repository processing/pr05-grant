# Monthly Report - August: Incremental Typescript Migration for the p5.js Web Editor

## Overview

Over August, the PRs from July were reviewed, and received really useful feedback to action. As such I spent a great portion of the month going back to old PRs, updating to address feedback, and updating new PRs to use the learnings from these previous PRs.

In addition to the PRs from July, I have additionally opened PRs to:
- Migrate the entire client/components folder
- Migrate the root files related to redux, and migrate an instance of a redux reducer system (preferences system) -- draft PR

As of the end of August, the following have officially been merged:

<img src="./pr05_2025_August_report_Claire_Peng_closedPRs.png">


## Key Decisions and Progress:

- Update type definitions to use `interface` instead of `type`
- Update 

## Challenges and Solutions:



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

