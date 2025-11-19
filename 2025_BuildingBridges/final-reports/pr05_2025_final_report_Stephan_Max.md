# Final Report: Desktop Support for p5.js via a New Processing Mode

## Overview

- Project
  - [Description](https://github.com/processing/pr05-grant/wiki/2025-pr05-Project-List#desktop-support-for-p5js-via-a-new-processing-mode)
  - Period: July–October 2025
- People
  - Grantee: [Stephan Max](https://stephanmax.com)
  - Mentor: [Stef Tervelde](https://github.com/Stefterv)
- Links
  - [Repository](https://github.com/stephanmax/processing-p5.js-mode)
  - [Wiki](https://github.com/stephanmax/processing-p5.js-mode/wiki)
  - Links point towards by personal GitHub page. The mode *will* move to the official Processing organization on GitHub and GitHub maintains redirects whenever repository ownership is transferred, so there should be now disruptions.

## Technical Decisions

### General

This mode has been developed in Kotlin and all mode-specific implementations can be found in the top-level [p5js](https://github.com/stephanmax/processing-p5.js-mode/tree/pr05-poc/p5js) folder. Necessary build steps (general build, generate `pdex` file for distribution, in the future: download p5.js examples) have been specified with Gradle. Hence, this mode complies with the general implementation direction of Processing.

We decided to go with Electron as our embedded browser as it is a mature product with a big user base and excellent documentation. We are aware that there are competing products out there, most notably Tauri, and we will keep on monitoring them as we move forward with this mode.

### Node Package Management

We chose [PNPM](https://pnpm.io) as the underlying node package manager. PNPM stores dependencies and version diffs in a central store and allows dependencies across sketches, for example Electron, to be reused. That way we save a lot of space and installation time. You can read more about the decision process [in the wiki](https://github.com/stephanmax/processing-p5.js-mode/wiki/Chosen-Technologies).

Bonus: We realized that `pnpm` can also be used as a Node version manager which solves a second issue for us. `pnpm` is therefore our one-stop-shop for all things Node required by the mode.

### Packaging Distributable Apps

We evaluated both [Electron Forge](https://www.electronforge.io) and [electron-builder](https://www.electron.build/index.html) for turning p5.js sketches into distributable apps. Although the former seems to be the officially endorsed solution by Electron, its configuration has been proven to be complicated and interop with `pnpm` at least complicated. `electron-builder` has worked almost out-of-the-box and we managed to export sketch apps for Linux, MacOS, and Windows.

### Distribution of the Mode

We created a Gradle task that builds the mode into installable `pdex` files that have been distributed as [beta relases on GitHub](https://github.com/stephanmax/processing-p5.js-mode/releases). By the end of November, this mode will appear in the official [contributions manager](https://github.com/processing/processing-contributions) of the Processing Development Environment.

## Challenges

- A lof of new things to learn: Kotlin (especially concurrent programming, the mix of object-oriented and functional programming, and Java interop), Gradle (in Kotlin), Compose Multiplatform for UI, and some Electron intricacies.
- Reversing my usual modus operandi—getting all the requirements first and then execute the implementation—to getting a product, that we as developers think will be helpful and meaningful, in front of the actual users as soon as possible and derive the actual requirements from them. That was a new spin for me and sometimes collided with my general tendency to drill deep into technical issues rather than work broadly to generate as much value as possible in a restricted timeframe.
- The mode relies on a critical amount of moving parts to get everything working: `pnpm` for package and Node management, Node JavaScript runtime for basic inner workings, Electron to display sketches, `electron-builder` for exporting sketches as executables. Getting all these tools in place on all relevant operating systems and play nice with each other has been challenging.
- Personal struggles impeded the timeline.

All these challenges—especially the last one—have been met by Stef Tervelde and Raphaël de Courville with patience, understanding, and the necessary amount of project management support to push things over the finish line.

## Completed Tasks

- [x] Editor Experience
  - [x] Communication between Electron and Processing via Electron’s inter-process and standard I/O process communication
  - [x] Baseline: Run/Stop buttons, sketch size as per `createCanvas()`, hide non-js and Electron boilerplate
  - [x] Syntax highlighting; we are aware that the underlying system for syntax highlighting has its flaws as is likely to change in the future. You can read more on syntax highlighting [in the wiki](https://github.com/stephanmax/processing-p5.js-mode/wiki/Syntax-Highlighting).
  - [x] Examples
  - [x] Error reporting
- [x] Package installation via pnpm
- [x] Auto-install all necessary dependencies (pnpm, Node, Electron) on Linux, MacOS, and Windows
- [x] Packaging sketches as distributable apps for Linux, MacOS, Windows
- [x] Proof-of-concept TypeScript example

## Limitations and Next Steps

I would like to fix some unpleasant bugs in the remainder of November and have the mode moved to the Processing GitHub organization and into the contribution manager by the end of November. These bugs are mainly around general stability and addressing some error messages that we have encountered. [Issues are tracked via GitHub.](https://github.com/stephanmax/processing-p5.js-mode/issues)

### Other Work Remaining

- [ ] Better TypeScript example; include all official p5.js examples
- [ ] Quality-of-life improvements around app export (icon, app title, non-hardcoded export settings)
- [ ] Button to open developer tools in Electron’s Chromium browser
- [ ] Add JavaScript linting to error reporting and allow errors/warnings to show without running the sketch first
- [ ] Remove package installation UI for now and rather add a menu action for user’s to edit `package.json`

## Closing Thoughts

This grant was an exciting and empowering experience from start to finish. I felt the right amount of challenge outside of my comfort zone and appreciation of my talents. The level of professional and personal learning for me is unprecedented. I am grateful for all the people involved and for being a part of this warm-hearted community. Thank you, Processing Foundation!
