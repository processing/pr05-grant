# August Monthly Report: Revamp the Friendly Error System (FES) in p5.js

By Miaoye Que, mentored by Kenneth Lim and Dave Pagurek with support from Qianqian Ye.

## Work Completed

### Accidentally Overridden Constants and Functions

- [x] Picked out a parser for detecting overridden constants and functions (`Espree`).

### Parameter Validation

To see all PRs and real-time progress for parameter validation, check out the [master issue #7178](https://github.com/processing/p5.js/issues/7178).

- [x] Start a new parameter validation file using Zod for p5.js.
- [x] Update script that generates `parameterData.json` to further reduce the size of the file.
- [x] Validate against real p5 constants.
- [x] Handle schema generation for optional parameters.
- [x] Implement a distance calculation mechanism similar to [`scoreOverload`](https://github.com/processing/p5.js/blob/ada3c05da35bb8daf3b9d0ca1dc5713bff0b95c1/src/core/friendly_errors/validate_params.js#L425) that gives users the proper error message when parameter validation fails.
- [x] Start a test file for new parameter validator.
- [x] Fix the case where a parameter can be a mix of constants and primitive types.

## Challenges Encountered

### General

- At the end of July, I planned to set August as my implementation-heavy month, with the aim of getting both of the priorities for the FES project implemented or almost implemented. However, I ended up spending most of my time getting TypeScript exporting to work. Later, I realized that some parts of my plan, although conceptually ideal, might be more complicated and uncertain than I expected when it comes to implementation. To this end, I'm letting myself adjust my timeline a bit, to be more realistic with the amount of work that needs to be done.
- More generally, I initially divided my grant period into clearly-defined, task-oriented milestones (i.e. research first, then implementation, ending with testing and integration). In reality, the process of coding and implementation often reveals oversight and issues that weren't apparent during the research phase. For future projects (both mine and other Processing Foundation associates'), it's better to envision grant periods in terms of _iterations_, instead of _phases_.

### Accidentally Overridden Constants and Functions

- I spent minimal time on this task in July, so there's nothing much to report here.

### Parameter Validation

- Approach: leverage on `zod`, a TypeScript-first schema validation library with static type inference.
- Planned workflow: generate `.d.ts` file -> use the generated file as input in `ts-to-zod`, generate `zod` schema -> use the generated schema for parameter validation.
  - `.d.ts` file generated is currently too big and has many duplicated elements.
  - Methods that are chainable and return the p5 instance inline an interface for the whole p5 module as its return type.
  - Unsure if we can get clean TypeScript types; this might be a dead end that we simply haven't figured out yet, and it'd require a lot of work to have a clear answer.
- New workflow: based on the JSON file that contains all the p5 methods and their accepted parameters, write a custom parser that generates `zod` schema for validation.
  - Writing a custom parser means that we need to explicitly define each parameter type and constant, which turned out to be rather complicated and requires a lot of attention to detail.
  - For object (both p5 objects such as `p5.Camera` and Web API objects such as `Audionode`) validation to work, I need to bring up an actual browser environment, which is more complicated than just runnign `node file.js` for testing.

## September Plans

- Wrap up the implementation, testing, and integration for parameter validation.
- Start working on the next priority: report accidentally overridden constants and functions; at least get to a 75% done state.
