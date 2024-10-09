# September Monthly Report: Develop a VSCode LSP Server Extension for Processing
By **Diya Solanki**

- [Project Repository: Processing Language Server Extension](https://github.com/diyaayay/processing-language-server-extension/)
- [Working Branch: main](https://github.com/diyaayay/processing-language-server-extension/tree/main)
- [Testing Branch: test](https://github.com/diyaayay/processing-language-server-extension/tree/test)

## Completed Tasks (what's done so far in September)

- [x] [Test Branch Setup](https://github.com/diyaayay/processing-language-server-extension/tree/test) A test branch was created to install and run the extension on any platform, including the ability to run Processing sketches on Windows, along with LSP features. Clone the test branch to try out the Processing LSP Extension.
- [x] [Project Setup Simplification](https://github.com/diyaayay/processing-language-server-extension/tree/test?tab=readme-ov-file#installation) Simplified the project setup process to use minimal commands, improving accessibility for users.
- [x] [Project Dependencies release](https://github.com/diyaayay/processing-language-server-extension/releases/tag/v1.0) Downloads the Processing sketch-running dependencies on setting up the extension locally.
- [x] [Deps download script](https://github.com/diyaayay/processing-language-server-extension/blob/test/dependency.bat) A script was created to run before `npm install`, automating the download and extraction of dependencies.
- [x] [On-hover Description](https://github.com/diyaayay/processing-language-server-extension/tree/test?tab=readme-ov-file#on-hover-description) Hovering the mouse over a keyword now displays a pop-up with the meaning of the keyword.
- [x] [Go-to Definition](https://github.com/diyaayay/processing-language-server-extension/tree/test?tab=readme-ov-file#go-to-definition) The "Go to Definition" feature allows users to navigate to the definition of a symbol using cmd+click or by right-clicking the variable name and selecting `Go To Definition`.
- [x] [Go-to References](https://github.com/diyaayay/processing-language-server-extension/tree/test?tab=readme-ov-file#go-to-references) The references request is sent from the client to the server to resolve project-wide references for the symbol denoted by the given text document position. This feature finds all the references of a variable, class or a function used.
 Users can right-click or click on the `n` References link that appears over declarations.
- [x] [Code Completion](https://github.com/diyaayay/processing-language-server-extension/tree/test?tab=readme-ov-file#code-completion) Context-aware suggestions are provided to speed up coding.
- [x] `Running Processing Sketches Headless`: The `ctrl+shift+p` command or the `Run` button in the top right corner runs Processing sketch in the user's system.


## Challenges Encountered
- **Diagnostics** for Processing sketches from Processing’s sketch validation logic is still a work in progress.
- **Running Processing headless** without requiring the user to set paths or require some complex setup was the main challenge to make the version migration work everywhere.
- **Dependency Management:** Handling external dependencies through symlinks and scripts proved tricky. Ensuring that the right libraries were accessible, while maintaining compatibility with different system environments, added complexity.
- This required careful handling of directory structures, ensuring each symlink pointed to the correct target without causing confusion or overlap.
- **Error Handling in Symlink Creation:** Error handling for symlink creation was important to ensure that if a symlink already existed or failed to be created, the system handled it gracefully without halting the entire setup process. 
- After setting up the script to download and unzip the dependencies, integrating them in the `sketchRunner.ts`, and testing them locally, I set up a Windows VM to clone the test branch and ensure that the extension is working on other machines.
  

## Plans for October

- [ ] `Goal`: Bug Fixes in:
    - [ ] Code-completion
    - [x] Hover (no known bugs)
    - [x] Syntax highlighting (no known bugs)
    - [ ] `onReferences`
    - [ ] `onDefinition`
    - [ ] `...`
  
- [ ] Enhance the UI for sketch/sketchbook and running the sketches.
- [ ] Export the extension into a `.vsix` file and publish it to the [VSCode Market Place](https://marketplace.visualstudio.com/vscode).
- [ ] Detailed documentation for further development of the project by new contributors.
- [ ] Detailed documentation for installing and configuring the extension.