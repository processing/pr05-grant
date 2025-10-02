# Monthly Report – September: Desktop Support for p5.js via a New Processing Mode

## Overview

The goal of this month was to improve the editor experience of the p5.js mode and make it more robust and stable. This included proper syntax highlighting for p5.js keywords, error reporting inside the Processing Development Environment, and auto-installing all the necessary tools and dependencies—pnpm, Node, and Node packages—to get a p5.js sketch running inside Processing. The latter is especially important, as it is a cornerstone of Processing’s accessibility that we do not assume the presence of any software on the user’s machine. Processing—and any mode, for that matter—should be batteries-included.

## Key Decisions and Progress

### Syntax Highlighting

I generated keywords for p5.js based on the official reference JSON and for JavaScript based on Codemirror. That way, we stay as close to the implementation of syntax highlighting of the p5.js web editor as possible and hopefully cause no confusion for people that work in both environments. However, the current implementation of syntax highlighting via KEYWORDS.txt has its issues: It does not seem to be context-sensitive, so highlighting the property `.width` and the top-level variable `width` differently is not possible. That also leads to the wrong highlighting of the math function `log()` inside `console.log()`. It is also not clear what a JavaScript keyword is and how far down the official ECMAScript spec one wants to go. After a chat with my mentor Stef and considering the ongoing efforts to move Processing to a more robust LSP-based solution, we decided to not invest more time into a better solution for now. You can read more about how I generated the keywords in the [wiki](https://github.com/stephanmax/processing4/wiki/Syntax-Highlighting).

### Error Reporting

Sketch errors happen inside the renderer processes of Electron where they are caught by implementing listeners for `error` (`throw`n errors) and `unhandledrejection` (rejected/unfulfilled `Promise`s) events. p5.js uses the same logic to implement its [Friendly Error System](https://p5js.org/contribute/friendly_error_system/). Errors are parsed and sent to the main process of Electron where they are printed to standard output and recorded by the PDE. I use `SketchException`s to report the error in PDE’s status bar and highlight the error inside the code editor.

### Auto-Install

This month I started to add [releases](https://github.com/stephanmax/processing4/releases) as `.pdex` files. Double-clicking will install the mode inside Processing. Once the user restarts Processing, the mode will check for `pnpm` and `node` and install both if not found. Once all Node dependencies are installed, the user can go ahead with working on their sketch. This has been tested with Processing 4.4.7 on MacOS Monterey 12.7.6.

## Challenges and Solutions

This month has been scattered with personal and professional challenges and I want to thank both Stef and Raphaël for providing me with the necessary order and support to help me get this mode into a serviceable state.

The biggest challenge of this milestone was the auto-install, as it not only required me to test the automated runs of command line processes on different platforms, but also to restore an environment without `pnpm` and `node` after every test to guarantee that the whole setup and install process works according to plan.

## Next Steps

Month and milestone 4 are about improving the mode’s stability further, adding more examples (most notably a TypeScript example to showcase more complex JavaScript tooling), adding a stand-alone app export feature, and releasing the mode as an official contribution for Processing’s contribution manager to install. I will keep the [p5.js mode README](https://github.com/stephanmax/processing4/blob/pr05-poc/p5js/README.md) updated with current features and remaining todos.

## Relevant Resources

- [Project repository](https://github.com/stephanmax/processing4/tree/main) with the [working branch](https://github.com/stephanmax/processing4/tree/pr05-poc)
  - [README](https://github.com/stephanmax/processing4/blob/pr05-poc/p5js/README.md)
- [Project wiki](https://github.com/stephanmax/processing4/wiki)

### Recommended Learning

- [Kotlin From Scratch](https://nostarch.com/kotlin-scratch) by Faisal Islam is a project-based, playful, creative, yet surprisingly thorough introduction to Kotlin.
