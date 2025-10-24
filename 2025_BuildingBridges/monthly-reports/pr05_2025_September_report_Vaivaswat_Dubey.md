# September Monthly Report: Visual Regression Testing Implementation

By **Vaivaswat Dubey**

[Current Implementation](https://github.com/Vaivaswat2244/processing4/tree/pr05-visualtests/visual-tests)

## System Architecture Progress
Following the custom Kotlin implementation of the comparison algorithm in August, the focus for September was on building a complete, integrated visual regression testing framework for the Processing environment. The architecture now encpsulates sketch execution, image comparison and gives result.

The core components now include:

- VisualTestRunner: The central orchestration class that manages the entire test lifecycle, from capturing a sketch to comparing it against a baseline image and generating a final test result.
- ImageComparator: The custom, self-contained pixel matching engine that uses a pixelmatch-inspired algorithm implemented entirely in Java, eliminating external dependencies.
- SketchRunner: A dedicated PApplet instance that manages the execution of a user's sketch in an off-screen buffer, ensuring consistent, repeatable rendering for testing purposes.
- ProcessingTestSuite: A class for organizing and running multiple tests, simplifying the management of large test suites.
- BaselineManager and TestExecutor: Utility classes to streamline baseline creation and single-test execution, making it easier for developers to manage their visual tests from the command line.

## Work Accomplished
### Processing Testing Framework Implementation

Successfully developed and integrated a comprehensive testing framework that allows developers to write and run visual tests for their Processing sketches with minimal boilerplate. This framework leverages all the components planned in the architectural phase and provides a complete solution from sketch to final report.

Technical Implementation Details:

- Sketch Isolation: The SketchRunner class provides a controlled environment for executing user sketches. It runs each sketch as its own PApplet instance, captures the final rendered image, and ensures consistent size() and noLoop() settings, which are critical for reproducible visual output.
- Test Orchestration: The VisualTestRunner class was completed to handle the full testing workflow. It now correctly identifies first-time runs to create a baseline image, loads existing baselines for comparison, and, if a test fails, automatically saves a diff image for debugging purposes. This simplifies the developer's debugging process.
- Pixel-by-Pixel Comparison: The custom ImageComparator now performs a thorough pixel comparison, using an algorithm that is a functional equivalent of the pixelmatch library. It correctly identifies differences, post-processes them to find clusters of changes, and can intelligently ignore minor, isolated pixel differences or "line shifts" that might occur due to rendering subtleties.
- Simple Test Class: To check the workability of the SketchRunner, whipped up a class containing some basic tests such as different coloured squares and a gradient sketch. In future this will be replaced by `JUnit`. Similar workarounds will be made for the suite execution which is supposed to group selected tests together and then run them as a suite.

### Integration Testing and Validation

Extensive testing was performed using a some simple tests, including shapes, gradients. This validation process confirmed that the framework correctly:

- Creates Baselines: The system successfully creates __screenshots__ directories and saves new baselines on the first run.
- Detects Changes: A modified sketch (e.g., changing a red circle to a green one) correctly fails the test and generates a diff image.
- Handles Identical Runs: Running the same sketch multiple times with an existing baseline correctly results in a "PASSED" status.
- Manages Minor Differences: The algorithm's ability to identify and filter out insignificant "line-like" clusters was validated, ensuring that minor anti-aliasing artifacts do not cause false failures.

## Challenges Encountered
The primary challenge in September was managing the complexities of the Processing runtime itself. PApplet is not designed to be run non-interactively, so controlling its lifecycle and ensuring that it renders a single frame before exiting required careful use of noLoop(), runSketch(), and a robust waiting mechanism. The thread-based nature of PApplet.runSketch() required a separate synchronization mechanism to ensure the main test thread does not proceed until the sketch has fully rendered its output, which was a key aspect of the SketchRunner implementation.

Additionally, ensuring the custom ImageComparator was robust enough to handle the variety of visual output, from simple shapes to complex, randomized patterns, required careful tuning of the clustering and lineShift detection logic to prevent false positives while still catching real regressions.

## Key Insights
- Headless Processing is a non-trivial problem: The PApplet class is inherently tied to a visual window. Running it in a "headless" or non-interactive mode for testing purposes required a bespoke solution to manage the application lifecycle and capture the rendered output.
- Custom pixel matching provides full control: The decision to implement a custom, Kotlin-based comparison algorithm has paid off by providing a completely self-contained solution. It avoids all the complexities and trade-offs of the binary investigation from August and gives us granular control over every aspect of the comparison logic.

## October Plans
October will focus on polishing the existing implementation and preparing for a public release.

### Documentation and Examples:

- Using JUnit rather than the independent classes being used right now.
- Porting the visual tests currently in p5.js for Processing.
- Write clear, easy-to-follow documentation and tutorials on how to set up and use the framework with a standard Processing sketch.
- Develop a variety of example sketches that demonstrate different features of the tool, including how to handle different test configurations.

The primary deliverable for October will be a public, well-documented, and stable version of the visual regression testing library for Processing, ready for use by the community.