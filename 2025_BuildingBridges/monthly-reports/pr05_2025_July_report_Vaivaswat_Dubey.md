# July Monthly Report: Snapshot Testing for Processing and p5.js
By **Vaivaswat Dubey**

- [Project Repository: Processing Comparison Engine](https://github.com/Vaivaswat2244/processing-comparison-engine/)

## System Architecture and Shared Components

### How the System Works

The proposed architecture centers around a shared comparison engine that both Processing and p5.js can utilize through platform-specific adapters. Rather than forcing either ecosystem to change their existing workflows, each platform maintains its current testing approach while leveraging a common underlying image comparison service.

### Shared Components from p5.js

The core comparison functionality will be extracted from p5.js's existing visual testing infrastructure and packaged as a reusable NPM module:

* **Proven pixelmatch implementation** - The existing pixelmatch algorithm setup that p5.js currently uses for visual regression testing, including all the threshold logic and decision-making around what constitutes a meaningful visual difference.

* **Threshold management system** - The established approach for handling false positives and determining acceptable levels of visual variation, which has already been tested and refined in the p5.js ecosystem.

### Platform-Specific Adapters

Instead of creating a monolithic system, thin adapter layers will handle ecosystem-specific integration:

* **Java adapter for Processing** - Utilizes gradle-npm plugin to interface Processing's Gradle-based build system with the NPM-packaged comparison engine, allowing Processing libraries to perform visual testing without changing their existing build workflows.

* **JavaScript adapter for p5.js** - Works within the existing Vitest testing framework, serving as a bridge between p5.js's current test structure and the shared comparison engine.

This adapter approach ensures that improvements and bug fixes to the core comparison logic benefit both ecosystems, while allowing each platform to evolve their testing approaches independently without breaking the other.

## Work Accomplished

The main goal this month was to explore and implement proof-of-concept integration solutions between NPM and Gradle build systems, while researching containerization alternatives and binary development approaches. These specific approaches were selected based on key design criteria: minimizing contributor friction, maintaining compatibility with existing Processing and p5.js workflows, and enabling shared development efforts between the two ecosystems. Initial work focused on understanding existing integration patterns in the community and evaluating different technological approaches for my specific use case.

### NPM-Gradle Integration Proof of Concept

I successfully developed a working proof of concept for NPM-Gradle integration that demonstrates seamless coordination between JavaScript/Node.js and JVM-based build processes. This proof of concept addresses the common challenge of managing hybrid application dependencies across different package managers. NPM was ultimately chosen as the preferred approach due to its significantly lower contributor setup friction compared to containerized alternatives.

The integration developed includes:

* **Custom Gradle tasks integration** - Created automated Gradle tasks that invoke NPM commands during the build lifecycle, ensuring frontend dependencies are resolved and assets are compiled as part of the standard Gradle build process. This eliminates the need for developers to manually run separate NPM commands and reduces the chance of build inconsistencies.

* **Unified dependency management** - Implemented coordination between package.json and build.gradle files so that both JavaScript and JVM dependencies are managed through a single build command. This includes automatic detection of dependency conflicts and version alignment between the two package management systems.

* **Cross-platform build compatibility** - Developed build scripts that work consistently across Windows, macOS, and Linux development environments without requiring platform-specific configuration. This ensures that all team members can contribute regardless of their operating system choice.

### Gradle Setup Research

Conducted extensive research on optimal Gradle configurations for modern development workflows, focusing on build performance optimizations and plugin ecosystem compatibility. Key findings included the importance of proper daemon configuration, build caching strategies, and plugin selection for hybrid JavaScript/JVM projects.

### Docker Alternatives Investigation  

Researched containerization technologies including Docker. However, after evaluation, the Docker approach was ultimately declined due to the significant contributor friction it would introduce. Requiring contributors to learn container technologies and manage containerized development environments would create unnecessary barriers to community participation and make the contribution process more complex than necessary.

### Binary Development with Go and Rust

Comprehensive analysis was performed comparing Go and Rust for binary development, evaluating compilation speed, runtime performance, memory management, and development velocity. However, questions arose regarding how well these binary approaches would integrate with the existing Processing environment and whether they would create additional complexity for library developers who are already familiar with Java-based workflows.

### Comparison Engine Development

I began development of a comparison engine using pixelmatch, a JavaScript library for precise pixel-level image comparison. This work includes:

* Established the foundation for automated visual testing by integrating pixelmatch into my testing infrastructure. This JavaScript library provides precise pixel-level comparison capabilities that can detect even subtle visual differences in Processing library outputs, making it ideal for regression testing and quality assurance workflows where images are generated directly in p5.js and Processing environments.

* Implemented the algorithm which is already in place in p5 with some changes to work with the npm structure.

## Challenges Encountered

The main challenge met in this work was navigating the complexity of Gradle's plugin system and build lifecycle. The documentation was often unclear about what specific plugins accomplish, particularly when trying to implement solutions using Kotlin DSL instead of the traditional Groovy syntax. I encountered unexpected difficulties where builds would succeed in IntelliJ's Gradle integration but fail when executed from the command line, requiring investigation into classpath and environment differences.

Another significant challenge was balancing technical capabilities with contributor accessibility. While containerized solutions offered better isolation and reproducibility, they would require contributors to learn additional tools and concepts. Similarly, while Go and Rust offered performance benefits for binary development, their integration with the Processing ecosystem remained unclear and could potentially fragment the development experience.

## Key Insights

Through my research and development work, several important insights emerged:

* **Contributor friction is a critical factor** - Technical solutions must be evaluated not just on their capabilities but on how they impact the ease of contribution. NPM integration was preferred over Docker specifically because it requires less setup and learning from contributors.

* **NPM-Gradle integration provides the best balance** - This approach offers improved build automation and dependency management while maintaining familiar workflows for developers already working with JavaScript and Java technologies.

* **Processing environment integration concerns** - Binary development approaches using Go and Rust, while technically promising, raise questions about how well they would integrate with Processing's existing Java-based ecosystem and whether they would create unnecessary complexity for library developers.

* **Build system complexity requires careful documentation** - The fragmented nature of Gradle documentation, particularly around Kotlin DSL usage, creates barriers that must be addressed through comprehensive setup guides and examples.

## August Plans

The plans for next month are focused on two main areas:

1. **Comparison Engine and Testing Infrastructure** - I will continue development of the pixelmatch-based comparison engine which will involve testing it with the images generated in the p5 and processing environment. This includes establishing tolerance thresholds and acceptance criteria for visual differences, determining what percentage of pixel differences should trigger test failures versus passes, and creating a standardized approach for handling false positives in automated visual testing.

2. **Adapter Development** - I plan to complete the initial implementation of both the processing and p5 adapter which involve building the visual testing infrastructure for both the project. Given that p5.js already uses Vitest and has integrated visual tests, while Processing will require a complete setup from scratch, I will prioritize the Processing adapter development to establish the foundational infrastructure needed for that ecosystem.

In August I will be finishing the comparison engine implementation and will start testing these automated comparison systems with actual Processing and p5.js development workflows to ensure they can effectively catch regressions and visual inconsistencies.