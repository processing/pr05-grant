# September Monthly Report: Revamp the Friendly Error System (FES) in p5.js

By Miaoye Que, mentored by Kenneth Lim and Dave Pagurek with support from Qianqian Ye.

## Work Completed

### Accidentally Overridden Constants and Functions

To see all PRs and real-time progress for accidentally overriden constants and functions, check out the [master issue #7269](https://github.com/processing/p5.js/issues/7269).

- [x] Started a new `sketch_verifier` file that will detect accidentally overriden constants and functions.
- [x] Script can now retrieve user's code as a string and with the help of `Espree` (parser), identify user-defined functions and constants.

### Parameter Validation

To see all PRs and real-time progress for parameter validation, check out the [master issue #7178](https://github.com/processing/p5.js/issues/7178).

- [x] Parameter validation now validates against real p5 (i.e. `p5.Color`) and web API objects (i.e. `KeyboardEvent`).
- [x] Updated `convert.js`, script that generates `parameterData.json` so that 1) we ignore false definitions and 2) deduplicate overloads with the same order and types.
- [x] Implemented the new modular syntax and dependency injection pattern for parameter validation.
- [x] Parameter validation now handles array-type arguments dynamically, i.e. identifies that an argument's type ends with `[]` and applies `z.array()` to object type.
- [x] Instead of using `zod-validation-error` library, now prints error messages with custom logic catered towards beginners.
- [x] Updated test file to cover all test cases from the old test file and then some.
- [x] Added parameter validation support for paletteLerp, see [#6960](https://github.com/processing/p5.js/issues/6960).
- [x] Some code cleanups now that the parameter validation is almost at completion.

## Challenges Encountered

### General

- Again, I struggled with time management because I started a full-time job early September. I was working at a pretty erratic cadence. Fortunately, I'm only a little behind schedule so this was not a major concern.

### Accidentally Overridden Constants and Functions

- Had trouble getting the user's code as a string and needed help from Ken and Dave.

### Parameter Validation

- I don't think I faced challenges for parameter validation per se, but what I thought would be done swiftly turned out to require more of my care and attention, and follow-up issues kept emerging. For example, after I finished the initial implementation, we realized that the messages printed were not beginner-friendly because of how technical they were. So the approach I thought would save me time (leveraging on an existing library `zod-validation-error`) turned out to be inappropriate for the task, and custom logic was required after all.

## Remaining Tasks

### Accidentally Overridden Constants and Functions

- [ ] Check if user-defined functions and variables have already been defined in `p5` and shouldn't be re-defined.
- [ ] Generate friendly messages for re-definitions that shouldn't take place.
- [ ] Write comprehensive testing for the feature.
- [ ] Integrate it into the `dev-2.0` branch.

### Parameter Validation

- [ ] Update friendly messages again if necessary.
- [ ] Integrate it into the `dev-2.0` branch.

## October Plans

- Wrap up the implementation, testing, and integration for both priorities.
- Update documentation, wherever applicable.
- Prepare for project wrap-up and presentation, i.e. interview with Yoon, the final group presentation.
