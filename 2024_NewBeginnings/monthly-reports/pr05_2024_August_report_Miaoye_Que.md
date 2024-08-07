# August Monthly Report: Revamp the Friendly Error System (FES) in p5.js

## Work Completed

- [x] Picked out a parser for detecting overridden constants and functions (`Espree`).

## Challenges Encountered

### General

- At the end of July, I planned to set August as my implementation-heavy month, with the aim of getting both of the priorities for the FES project done or almost done. However, after the first week of August, where I spent most of my time getting TypeScript exporting to work, I realized that some parts of my plan, although conceptually ideal, might be more complicated and uncertain than I expected when it comes to implementation. To this end, I'm letting myself adjust my timeline a bit, to be more realistic with the amount of work that needs to be done.

### Accidentally Overridden Constants and Functions

### Parameter Validation

- Original approach: leverage on `zod`, a TypeScript-first schema validation library with static type inference.
- Original workflow: generate `.d.ts` file -> use the generated file as input in `ts-to-zod`, generate `zod` schema -> use the generated schema for parameter validation.
  - `.d.ts` file generated is currently too big and has many duplicated elements.
  - Methods that are chainable and return the p5 instance inline an interface for the whole p5 module as its return type.
  - Unsure if we can get clean TypeScript types; this might be a dead end that we simply haven't figured out yet, and it'd require a lot of work to have a clear answer.
- New workflow: based on the JSON file that contains all the p5 methods and their accepted parameters, write a custom parser that generates `zod` schema for validation.
