# October Monthly Report: Develop a VSCode LSP Server Extension for Processing
By **Diya Solanki**

- [Project Repository: Processing Language Server Extension](https://github.com/diyaayay/processing-language-server-extension/)
- [Working Branch: Development](https://github.com/diyaayay/processing-language-server-extension/tree/Development)

## Completed Tasks (what's done so far in October)

- [x] [.vsix GitHub Release](https://github.com/diyaayay/processing-language-server-extension/releases/tag/v1.0) Users can download the extension as a `.vsix` file to install in VSCode via `install from VSIX...`
- [x] [Bundling of the extension](https://learn.microsoft.com/en-us/visualstudio/extensibility/anatomy-of-a-vsix-package?view=vs-2022) Used `vsce package` to package the extension with its dependencies. The large size of the extension is due to custom dependencies downloaded via the `dependency.bat` script. The [dependency.zip](https://github.com/diyaayay/processing-language-server-extension/releases/download/v1.2/dependency.zip) contains the Processing distribution for Windows, which is extracted into `jre/deps`.
- [x] [VSCode MarketPlace Release](https://marketplace.visualstudio.com/items?itemName=DiyaSolanki.processing-language-server-extension) Users can download the extension from marketplace or VSCode's extension tab. Referenced [Testing and Publishing extensions](https://code.visualstudio.com/api/working-with-extensions/testing-extension) documentation.
- [x] [Sketchbook and multiple sketch runs](https://github.com/diyaayay/processing-language-server-extension?tab=readme-ov-file#tutorial) A sketchbook can contain multiple sketch directories, where the stop buttons stops `watching` a sketch and start another, [tutorial](https://github.com/diyaayay/processing-language-server-extension?tab=readme-ov-file#run-the-extension-in-windows).
- [x] [Logging Sketch Errors](https://github.com/diyaayay/processing-language-server-extension?tab=readme-ov-file#tutorial) Errors are logged in the "Processing Sketch" output tab.
- [x] Testing across machines for .vsix Packaging: Tested both GitHub and Marketplace versions on local machines and VMs.
- [x] Streamlined the test and development branches for easier collaboration and future contributions.
- [x] [Final Presentation](https://www.canva.com/design/DAGKm-rvxew/3XZsSO0fkGKFhEoQA_PxKg/edit?utm_content=DAGKm-rvxew&utm_campaign=designshare&utm_medium=link2&utm_source=sharebutton) Completed a final presentation with a live demo, including a recorded demo backup for the Code and Community presentations.
- [x] [Documentation](https://github.com/diyaayay/processing-language-server-extension/tree/test?tab=readme-ov-file#go-to-references) Wrote the [final technical report](2024_NewBeginnings\final-reports\pr05_2024_final_report_Diya_Solanki.md) and a wrap up medium article, ([draft here](https://medium.com/@diya.solanki.31/my-journey-with-pr05-processing-foundation-2e6c629d59da)).
- [x] Fixed unexpected behavior with GitHub releases bound to commits on the default branch; detailed in [challenges encountered](#challenges-encountered)
## Challenges Encountered and Research
- Researched into cross-platform compatibility of the LSP extension: the Linux and macOS distribution of Processing can be packaged into the extension for sketch runner on respective platforms through symlinks.
  -  We aimed to run the processing sketches inside the extension host (which is different from running the sketches in a JavaScript frontend, for example). We could simply call the binary using some `npm` package like `child_process`, `spawn-process`, etc., but then we need it inside the extension host as it should also have the client-server support - necessary to make all LSP features work smoothly.

- **Release binding with commit**: Encountered an unexpected issue with GitHub releases that took me some time to understand. When the maintainer creates a GitHub release, it automatically binds to a commit on the default branch. When this branch is cloned, it causes multiple issues, including connection failures with the LSP client and errors related to the VSCode API. After some research, I resolved the problem by binding GitHub releases to a commit on a branch other than the default branch.
## Achieved in October

- [x] `Goal`: Bug Fixes in:
    - [x] Code-completion
    - [x] Hover (no known bugs)
    - [x] Syntax highlighting (no known bugs)
    - [x] `onReferences`
    - [x] `onDefinition`
    - [x] `Sketch Error logging`

- [x] Development branch setup.
- [x] Sketchbooks and multiple sketch running
- [x] Export the extension into a `.vsix` file and publish it to the [VSCode Market Place](https://marketplace.visualstudio.com/vscode).
- [x] [Documentation](https://github.com/diyaayay/processing-language-server-extension?tab=readme-ov-file#build-instructions) for further development of the project by new contributors.
- [x] [Documentation](https://github.com/diyaayay/processing-language-server-extension?tab=readme-ov-file#run-the-extension-in-windows) for installing and configuring the extension.

## Future Plans
- Maintain the repository, feature requests, and address any bugs.
- Reach out to people to contribute to the project.
- As and when time permits, add processing distibution bundles for all platforms.
- Future goals for the project include:
   - Establish a comprehensive testing strategy with full feature tests, maybe using [vitest](https://vitest.dev/), [example tests](https://github.com/hyperjump-io/json-schema-language-tools/blob/main/language-server/src/features/semantic-tokens.test.ts)
   - Extend Processing's AST and AST-supported features.
   - Transition from the custom bundling of Processing with symlink creation for multiple platforms to a modular approach by shipping the core and pre-processor as Maven packages(WIP), and offering a comprehensive CLI interface for Processing and its Language Server, significantly reducing the extension size.
   - Enhance the extension's interface to provide an intuitive experience with labeled icons, separate run/stop toggle buttons for each sketch, and other improvements. Drawing inspiration from Antiboredom's [p5.vscode](https://github.com/antiboredom/p5.vscode).

While VSCode remains one of the most widely used code editors, extending support for this LSP to other editors, including JetBrains, Eclipse, and CodeMirror, can also be a future goal.