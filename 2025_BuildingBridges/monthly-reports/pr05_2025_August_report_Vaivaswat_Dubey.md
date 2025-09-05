# August Monthly Report: Visual Regression Testing Implementation
By **Vaivaswat Dubey**
- [Project Repository: Processing Comparison Engine](https://github.com/Vaivaswat2244/visual-regression-engine/)

## System Architecture Progress

Building on July's architectural foundation, August focused on implementing the core comparison engine and developing platform-specific adapters. The project has undergone some changes after discussions regarding the inclusion of external dependencies, aiming to avoid them where possible. After further discussion, a significant change was made to the project's approach. For this, the executable binaries option which was introduced earlier was explored in more detail. Finally, after extensive research, instead of creating a modular component used by both p5.js and Processing, a custom comparison algorithm was implemented directly in Kotlin for the Processing environment.

The core architectural components established now include a custom pixelmatch-based comparison engine with threshold management systems implemented in Kotlin for Processing and the reporting system involved.

## Work Accomplished

### Comparison Engine Implementation
Successfully completed the core comparison engine implementation. This would have been the foundational component that both Processing and p5.js would have utilized through their respective adapters, with a new focus on platform-specific implementations. You can have a look at that [here](https://www.npmjs.com/package/visual-regression-engine)

**Technical Implementation Details:**
*   **Pixelmatch integration** - Implemented the proven pixelmatch algorithm, adapting it to work within a package structure that can be consumed. The integration maintains the same threshold logic and decision-making processes that have been tested and refined in the p5.js ecosystem.
*   **Threshold management system** - Developed a configurable threshold system that allows developers to set acceptable levels of visual variation based on their specific testing needs. This system handles false positives by providing granular control over pixel difference tolerance, color variance thresholds, and anti-aliasing detection.
*   **Cross-platform image handling** - Implemented robust image processing capabilities that work consistently across different operating systems and image formats, ensuring that visual comparisons produce identical results regardless of the development environment.

### Binary Development Investigation and Technical Challenges
Conducted extensive research into binary development approaches for creating standalone executables that could eliminate Node.js runtime dependencies. This investigation was motivated by the desire to reduce contributor setup friction and create more portable testing tools. Have a look at the releases [here](https://github.com/Vaivaswat2244/visual-regression-engine/actions)

**Nexe Implementation Challenges:**
*   **Massive build overhead** - Nexe compilation required building the entire Node.js runtime, resulting in build times of approximately 40 minutes per compilation. This proved impractical for iterative development and CI/CD workflows.
*   **Parameter deprecation issues** - The build process failed due to deprecated parameters in the fetch mechanism used to download Node.js sources. The deprecated API endpoints caused silent failures that were difficult to diagnose, often resulting in corrupted or incomplete builds.
*   **Binary size concerns** - Even successful Nexe builds produced executables exceeding 100MB due to the embedded Node.js runtime, making distribution cumbersome for library developers who expect lightweight testing tools.

**Esbuild Compilation Limitations:**
*   **Runtime dependency persistence** - Despite esbuild's excellent bundling capabilities, the output JavaScript files still required Node.js runtime for execution, completely defeating the purpose of creating standalone binaries for reduced dependency management.
*   **Module resolution complexities** - Esbuild struggled with dynamic imports and certain Node.js built-in modules used by pixelmatch and image processing libraries, requiring extensive configuration workarounds that introduced additional maintenance overhead.

**pkg Package Challenges:**
*   **Deprecation status complications** - Although pkg is officially deprecated, investigation continued to ensure comprehensive evaluation of available options. The deprecated status raised concerns about long-term maintainability and security updates.
*   **Windows-specific build failures** - Encountered consistent compilation errors on Windows platforms, specifically related to native module compilation and path resolution. These errors proved particularly challenging to debug due to pkg's limited error reporting and the deprecated nature of the toolchain.
*   **Native dependency handling** - pkg struggled to properly bundle native dependencies used by image processing libraries, often resulting in runtime errors when the binary attempted to access missing native modules.

**Deno Runtime Evaluation:**
*   **Dependency ecosystem limitations** - Deno's approach to dependency management conflicted with the project's need to leverage existing NPM packages like pixelmatch. Converting to Deno-compatible imports would require significant refactoring of established, well-tested libraries.
*   **Node.js compatibility layer issues** - When attempting to use Node.js compatibility mode for NPM dependencies, Deno essentially required Node.js compilation anyway, eliminating the benefits of a standalone runtime and creating additional complexity without solving the original dependency problem.
*   **Library integration challenges** - The fundamental mismatch between Deno's security model and the project's need to integrate with existing Processing and p5.js toolchains created architectural conflicts that would require extensive workarounds.

### NPM to Kotlin Implementation of Comparison Algorithm

The changing of the project's revised scope involved replacing the `pixelmatch` NPM package with a custom Kotlin function that calculates the exact Euclidean distance for color differences using the formula `sqrt((dr² + dg² + db² + da²)) / 255.0`. This ensures a highly accurate and consistent comparison. The rest of the implementation, including threshold management, image scaling, and diff image generation, was then structured similarly to the original NPM-based design, but entirely within the Kotlin ecosystem. This allows for a completely self-contained solution for Processing, eliminating the need for Node.js runtime or NPM dependencies.

## Challenges Encountered

The primary technical challenge was the binary development investigation, which consumed significant development time while ultimately proving unviable for this project's requirements. Each binary compilation approach presented unique technical hurdles that made them unsuitable for the contributor-friendly, cross-platform solution needed for Processing visual testing.

Cross-platform testing revealed subtle inconsistencies in image rendering and file system behavior that required platform-specific handling within the adapters, particularly around font rendering differences and color space management between operating systems. Additionally, considerable thought was put into ensuring the Kotlin implementation of the pixelmatch algorithm would be as robust and accurate as its JavaScript counterpart.

## Key Insights

**Binary development trade-offs proved prohibitive** - The investigation into standalone binary creation revealed that current JavaScript-to-binary compilation tools introduce more complexity than they solve. The compilation overhead, platform-specific issues, and maintenance concerns outweigh the potential benefits of eliminating Node.js as a runtime dependency.

**Kotlin implementation for Processing allows for dependency-free solution** - Implementing the core comparison logic directly in Kotlin for Processing provided a clean, dependency-free solution that integrates natively with the Processing environment, avoiding the complexities of bridging between NPM and Gradle ecosystems.

**Image comparison threshold tuning is critical** - Automated visual regression testing requires careful threshold configuration to balance sensitivity with practicality. Different types of Processing sketches may require different tolerance levels for meaningful regression detection.

## September Plans

September will focus on completing the adapter implementations and beginning comprehensive testing with actual Processing development workflows. 
**Processing Implementation and Reporting:**
*   Develop comprehensive test suites for the Kotlin implementation to ensure reliability across different image types and scenarios.
*   Create Processing-specific testing framework integration that leverages the native Kotlin comparison engine.
*   Design and implement a reporting system for Processing that provides clear visual regression test results and integrates with Processing's development workflow.

**Integration Testing and Validation:**
*   Conduct extensive testing with real-world Processing libraries to validate the adapter functionality and identify integration issues.
*   Performance optimization and memory usage analysis for both adapters under typical development workflows.

The primary deliverable for September will be fully functional adapters that Processing developers can integrate into their existing testing workflows with minimal setup friction, along with comprehensive documentation and migration guides.