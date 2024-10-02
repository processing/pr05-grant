# July monthly report: Develop a VSCode LSP Server Extension for Processing
By Diya Solanki

- [Project repository](https://github.com/diyaayay/processing-language-server-extension/)
- [Working branch](https://github.com/diyaayay/processing-language-server-extension/blob/main)


## Completed Tasks (what's done so far)

- [x] Reviewed [Efratror's prior work](https://github.com/efratror/LS4P) on the LSP for Processing and explored the possibility of updating to Processing 4.1+ and 4.3 (that one works on `Processing 4.0.8b`)
- [x] Built Efratror's LSP for `Processing 4.0.8b` using `Java 8` and the deprecated VSCode language server extension protocol.
- [x] Set up a roadmap to track tasks and decide on the approach for the project.
- [x] Successfully transitioned to Processing 4.3 by generating a release zip file containing `core.jar` and setting up the JRE zip files for running Processing sketches locally.
- [x] Established that the LSP is running locally for all Processing sketches and began discussions to find the latest versions of classes required for `custom.jar`.
- [x] Updated all NPM dependencies to use the latest packages.
- [x] Using `JDK 17.0.8` and `node v20.15.1` (the current LTS)
- [x] Managed to make the working version of the new transition! (but I still need to figure out the most concrete way of integrating the latest release and that is mainly for the ending part of the program so that is being researched simultaneously!)


## Challenges Encountered

- Transitioning from `Processing 4.0.8b` to `Processing 4.3` was more complex than anticipated, requiring adjustments in dependencies and approach.
- The `custom.jar` file in the old project [repo](https://github.com/efratror/LS4P) configuration involves significant changes from earlier versions, and specific classes need to be handpicked and updated.
- In the latest version of processing, there are significant changes, e.g., `javafx` is not shipped with the core library now (it used to, earlier) so I compiled the new `javafx` (built it with `ant`, [reference repo here](https://github.com/processing/processing4-javafx/releases)) and integrated it with the latest `core.jar`.
- I was facing some path issues also (probably due to new JRE) that I resolved.
- I needed new `JRE` to make it compatible with the latest processing so I compiled it with the latest `core.jar` using [this repository](https://github.com/efratror/LS4P).
- The NPM Java package currently used is optimized for OpenJDK 7, whereas `Processing 4.3` works best with `JDK 17`, causing compatibility issues.



## Plans for August

- [ ] Now that `Processing 4.3` is integrated (as in running all kinds of sketches), the focus for August would be to figure out the most concrete way of integrating the latest release to be built on any system and build LSP features on the processing sketch document using the latest VSCode LSP extension API like code completion, hover, go-to definition, diagnostics, validate sketches, etc. 
- [x] The priority of choosing the features would be to start from the most essential features and release a version for testing. (Update 02/Sep/2024: I now have a [roadmap](https://github.com/diyaayay/processing-language-server-extension/blob/main/ROADMAP.md) for the project that has a tentative list of tasks!)
- [x] August would also be focussed on integrating the `Processing 4.3` specific keywords in the AST which is the key to developing the the features on the text Document.
- [x] Other features such as `Run and Stop` buttons in the extension host for ease of use.