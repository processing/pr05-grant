# Monthly Report - September: Incremental Typescript Migration for the p5.js Web Editor

## Overview

Over September, I pivoted from the previous work on migrating the `client` folder to concentrating on setting up migration for the `server` folder. This included:

- Setting up TS dependencies on the `server` folder
- Migrating the `server/routes` folder
- Migrating server files related to emailing capabilities
- Migrating the `User` model and `ApiKey` schema
- Starting to migrate the `User` controller

I also addressed feedback from last month's PR's for the `client` folder.

## Challenges, Progress & Key Decisions:

This month started with easy-wins and ended with some great challenges, as the first steps of `server` folder migration started with repeating some flows I had learned in the previous two months on the `client` folder migration; then ended with learning about the intricacies of `mongoose` Typescript syntax, which I am still continuing to learn about.

### Setting Up TS Dependencies on the `server` folder & initial migrations:

Following what I learned for the client migration and had already set up for the repo, setting up TS dependencies on the `server` folder went relatively smoothly. I made the following changes:

- Set up tsconfig for the `server` folder
- Add a `typecheck:server` command to `package.json`, and manually checked that `typecheck:server` flags type-errors on .ts and .tsx files, and that it checks the `server` folder independently of the `client` folder

I then tested my work by migrating `/server/utils/generateFileSystemSafeName`, chosen as it was a simple file to understand. Upon running the app locally, I found that there were additional configurations needed, as webpack and babel weren't able to automatically resolve files ending in `ts`. Resolving these required some research but were the steps below:

- Updating `nodemon` to hot-reload upon ts-file updates
- Updating `webpack` to resolve extensions with server folder modules
- Updating `root/index.js` configurations for `@babel/register` to resolve server files with `@babel/preset-typescript` where appropriate

## PR's:

- [pr05 Typescript #8: migrate client/components/Menubar/MenubarSubmenu](https://github.com/processing/p5.js-web-editor/pull/3623)

### Outstanding PRs:

- [pr05 Typescript #8: migrate client/components/Menubar/MenubarSubmenu](https://github.com/processing/p5.js-web-editor/pull/3623)
