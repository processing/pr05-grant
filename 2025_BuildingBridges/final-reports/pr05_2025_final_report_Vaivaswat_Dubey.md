# Visual Regression Testing Framework for Processing

**Author:** Vaivaswat Dubey  
**Duration:** July – October 2025

## Description
Over the course of four months (July–October 2024), I focused on creating a comprehensive visual regression testing framework for the Processing environment that enables automated detection of rendering changes across different platforms and code modifications.

Visual regression testing is critical for Processing libraries and core development, as even minor code changes can introduce subtle visual artifacts that are difficult to detect manually.  
This project delivers a complete, dependency-free testing solution that integrates seamlessly with Processing’s existing Java-based ecosystem and Gradle build system.

The framework implements a custom pixel-matching algorithm inspired by the proven pixelmatch library, adapted entirely in Java to avoid external dependencies. It provides automated baseline management, platform-specific screenshot comparison, intelligent difference detection, and comprehensive test reporting through JUnit 5 integration.



## Implementation

### Core Infrastructure Components

#### ImageComparator.java
- Custom pixel-matching algorithm implementing pixelmatch-inspired comparison logic  
- Sophisticated cluster analysis to distinguish significant changes from rendering artifacts  
- Configurable thresholds:  
  - MAX_SIDE = 400px  
  - MIN_CLUSTER_SIZE = 4  
  - MAX_TOTAL_DIFF_PIXELS = 40  
- RGBA color distance calculation using Euclidean distance:  
  sqrt((dr² + dg² + db² + da²)) / 255.0  
- Intelligent filtering of line shifts and anti-aliasing differences  
- Automatic diff image generation highlighting differences in red  
- Post-processing to identify and filter insignificant pixel clusters  

#### VisualTestRunner.java
- Orchestrates the complete test lifecycle from sketch execution to result reporting  
- Platform-specific baseline management with automatic naming (-darwin, -linux, -win32)  
- First-run baseline creation with intelligent detection  
- Automatic diff image generation and storage on test failures  
- Hierarchical screenshot organization in __screenshots__/ directories  
- ComparisonResult objects containing pass/fail status, mismatch ratios, and detailed metrics  

#### SketchRunner.java
- Executes Processing sketches in controlled, isolated test environments  
- Extends PApplet for native sketch execution  
- Critical pixelDensity(1) setting ensuring cross-platform consistency (solves Retina/HiDPI issues)  
- Single-frame capture after setup() and one draw() call  
- Automatic window cleanup with proper exit() handling  
- Configurable render wait times for complex sketches  
- Thread synchronization to ensure complete rendering before capture  

---

### Supporting Classes

- ProcessingSketch Interface – Defines standardized setup() and draw() methods for test sketches  
- TestConfig – Configuration management for width, height, background color, comparison threshold, and render timing  
- ComparisonResult – Encapsulates test results including pass/fail status, mismatch ratio, and diff image data  
- ComparisonDetails – Detailed comparison metrics including total pixels compared, mismatched pixels, and cluster information  
- BaselineManager – Utility class for bulk baseline image updates and management  
- TestExecutor – Simplifies single-test execution from command line or scripts  

---

### JUnit 5 Integration

#### VisualTest.java (Base Class)
- Provides assertVisualMatch() helper method for test assertions  
- Handles first-run baseline creation using Assumptions.assumeTrue()  
- Integrates JUnit assertions for proper test reporting  
- Tagged with @Tag("visual") for test filtering and organization  
- Supports custom test configurations per test method  

#### Test Suites
- ShapesTestSuite: Organizes shape drawing tests using @Suite and @SelectPackages  
- TypographyTestSuite: Groups text rendering and font tests  
- Suite-level configuration with @SuiteDisplayName and @IncludeTags  
- Enables targeted test execution:  


## Comprehensive Test Coverage

### Shape Drawing Tests (~17 tests)
- Polyline rendering (open and closed paths)  
- Contour drawing (single and multiple nested contours)  
- Triangle fans and triangle strips  
- Quad strips with proper vertex ordering  
- Catmull-Rom spline curves using curveVertex()  
- Cubic Bezier curves with bezierVertex()  
- Quadratic Bezier curves with quadraticVertex()  
- Point and line primitives  
- Render mode variations (POINTS, LINES, TRIANGLES, QUADS)  

### Typography Tests (~20+ tests)
- Dynamic font loading with createFont()  
- Parameterized text alignment tests (9 combinations using @ParameterizedTest)  
- Horizontal: LEFT, CENTER, RIGHT  
- Vertical: TOP, CENTER, BOTTOM, BASELINE  
- Multi-line text rendering with manual line breaks  
- Text sizing and scaling  
- Text leading (line spacing) adjustments  
- Text width measurement validation  
- Complex rendering scenarios (rotation, transparency, color fills)  
- PFont-specific method tests (textAscent(), textDescent())  

### Shape-Modes Tests (~15 tests)
- Covers all four shape modes — CORNERS, CORNER, CENTER, and RADIUS — across ellipse, arc, and rect primitives.
- Validates correct placement and scaling of shapes under different coordinate interpretations.
- Includes quadrant-based drawing to verify shape alignment across positive and negative coordinate spaces.
- Tests negative width and height cases to confirm expected flipping and mirroring behavior.
- Ensures ellipseMode, arc, and rectMode produce consistent results across modes.
- Uses fill and stroke variations to confirm rendering consistency and anti-aliasing stability.
- Employs single-frame deterministic rendering for reproducible visual baselines.


## Technical Challenges and Decisions Taken

### Challenge 1: Headless Processing Execution
**Problem:** PApplet is fundamentally designed for interactive, windowed execution. Running sketches "headlessly" for automated testing required solving several issues:
- PApplet's lifecycle expects user interaction and continuous draw loops  
- No built-in mechanism for single-frame capture  
- Thread management complexity when running multiple tests sequentially  

**Solution:** Developed an execution strategy in SketchRunner:
- Override settings() to configure size and pixel density before window creation  
- Use noLoop() in setup() to prevent continuous draw calls  
- Call redraw() exactly once to execute a single draw cycle  
- Implement thread synchronization with CountDownLatch to wait for render completion  
- Force window cleanup with surface.setVisible(false) and exit()  

---

### Challenge 2: Binary Development Investigation (August)

**Problem:** Explored creating standalone executables to eliminate Node.js dependencies and reduce contributor setup friction.

**Approaches Investigated:**

| Tool | Verdict | Reason |
|------|----------|--------|
| Nexe | Impractical | ~40-minute builds, large binaries |
| Esbuild | Partial | Required Node.js runtime |
| pkg | Deprecated | Platform build failures |
| Deno | Incompatible | Ecosystem mismatch |

**Decision:** Abandon binary compilation approaches in favor of native Kotlin implementation. The complexity and limitations of JavaScript-to-binary tools outweighed their potential benefits.



### Challenge 3: NPM to Kotlin Migration
**Problem:** The initial architecture planned to use the NPM pixelmatch library as a shared component between Processing and p5.js, but this introduced significant complexity.

**Solution:** Complete reimplementation of the comparison algorithm in Java/Kotlin:
- Analyzed pixelmatch source code to understand core algorithm  
- Implemented Euclidean distance color comparison  
- Added cluster analysis for filtering insignificant differences  
- Developed line-shift detection for anti-aliasing tolerance  
- Created platform-specific threshold management  

**Impact:**
- Zero external dependencies for Processing visual tests  
- Native integration with Gradle build system  
- Faster execution without Node.js overhead  
- Simplified contributor setup (no NPM installation required)  
- Full control over algorithm tuning for Processing-specific needs  

---

## Architecture Evolution
The project's architecture underwent significant evolution based on practical challenges and community feedback:

### **July: Initial Shared Component Vision**
**Plan:**  
NPM package shared between Processing and p5.js  

**Architecture:**  
- Core comparison engine as an NPM module  
- Platform-specific adapters (Java for Processing, JS for p5.js)  
- Gradle–NPM integration via [gradle-npm-plugin](https://github.com/node-gradle/gradle-node-plugin) 



### **August: Binary Investigation Phase**
**Exploration:**  
Standalone executables to eliminate Node.js dependency  

**Tools Evaluated:**  
- Nexe  
- pkg  
- esbuild  
- Deno  

**Outcome:**  
All approaches proved unviable due to:  
- Excessive build times and large binary sizes  
- Platform-specific build failures  
- Maintenance concerns with deprecated tools  
- Fundamental architectural mismatches  



### **September–October: Native Implementation**
**Decision:**  
Complete Kotlin/Java implementation for Processing  

**Benefits:**  
- Zero external dependencies  
- Native Gradle integration  
- Faster execution  
- Simplified contributor setup  
- Full algorithm control  

**Architecture:**  
- Self-contained comparison engine in Java  
- JUnit 5-based test framework  
- Gradle-native build configuration  
- Platform-specific baseline management  



This evolution demonstrates the importance of **practical validation over theoretical architecture**.  
The final native implementation provides a **superior developer experience** compared to the initial cross-platform shared component vision.


**Benefits:**
- Zero external dependencies  
- Native Gradle integration  
- Faster execution  
- Simplified contributor setup  
- Full algorithm control  

---

## Key Insights and Lessons Learned

1. **Contributor Friction is Critical**  
Technical solutions must be evaluated not just on capabilities but on ease of contribution. The native Java implementation requires only JDK installation (already required for Processing development), while the NPM approach would have required Node.js, NPM, and cross-ecosystem knowledge. Lesson: Minimize barrier to entry over technical elegance.  

2. **Headless GUI Applications Are Non-Trivial**  
Running PApplet non-interactively required deep understanding of its lifecycle, threading model, and window management. The "just run it without a window" approach doesn't work. Lesson: GUI frameworks make assumptions about interactivity that must be explicitly managed in automated testing.  

3. **Binary Compilation Has Hidden Costs**  
The investigation into Nexe, pkg, and other tools revealed that JavaScript-to-binary compilation is far from mature. Build times, binary sizes, platform inconsistencies, and deprecated toolchains make this approach impractical for most projects. Lesson: Sometimes the "boring" solution (native implementation) is the best solution. 

4. **Threshold Tuning is an Art**  
Finding the right balance for MAX_TOTAL_DIFF_PIXELS, MIN_CLUSTER_SIZE, and color tolerance required extensive experimentation with real sketches. Too sensitive = false positives, too lenient = missing real bugs. Lesson: Default thresholds should work for most cases, but allow per-test customization. 

5. **Documentation in Code**  
The typography test challenges (white canvas problem) highlighted the need for comprehensive setup examples. Creating font explicitly, setting fill in draw(), using BASELINE alignment - these aren't obvious from Processing documentation. Lesson: Test code serves as documentation; make it exemplary. 

---

## Future Goals and Enhancements

### Expand Test Coverage
- 3D rendering tests (box, sphere, lights, camera)  
- Image loading and manipulation tests  
- Pixel array manipulation tests  
- Shader tests  
- Video and camera tests  

### Performance Optimization
- Parallel test execution avoiding window conflicts  
- Algorithm optimization for large images  
- Smart baseline caching  

### Enhanced Reporting
- HTML diff report generation showing side-by-side comparisons  
- Threshold suggestion based on historical test data  
- Visual summary dashboard for test suites  

---

## Important Links and References

- Current Implementation: [processing4/core/test](https://github.com/Vaivaswat2244/processing4/tree/more-visual-tests/core/test/processing/visual)  
- Monthly Reports: [Project Timeline](https://github.com/processing/pr05-grant/tree/main/2025_BuildingBridges/monthly-reports)  
- NPM Package: [visual-regression-engine (archived, superseded by native implementation)](https://www.npmjs.com/package/visual-regression-engine)  

---

## Conclusion

The Visual Regression Testing framework for Processing represents a significant enhancement to the Processing development ecosystem.  
By providing automated, reliable visual testing with zero external dependencies, the framework enables developers to catch rendering regressions before they reach users.

The journey from the initial NPM-based architecture to the final native implementation demonstrates the importance of practical validation and community feedback in open source development.  
While the path included detours through binary compilation and cross-platform integration challenges, the final solution provides a robust, maintainable foundation for Processing visual testing.
