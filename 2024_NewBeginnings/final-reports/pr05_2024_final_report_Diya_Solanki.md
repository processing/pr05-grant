<h1 align="center">
pr05'24 Final Report
</h1>
<h1 align="center">Create an Official Processing VSCode Extension
</h1>

| **Project**     | [Processing Language Server Extension](https://github.com/processing/pr05-grant/wiki/2024-Project-List-for-%60pr05%60-=-Processing-Foundation-Software-Development-Grant#%F0%9F%85%BF%EF%B8%8F-create-an-official-processing-vscode-extension) |
|-----------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Grantee**     | [Diya Solanki](https://github.com/diyaayay)                                                                                                                                                                                                         |
| **Mentors**     | [Sam Lavigne](https://lav.io/), [Justin Gitlin](https://cacheflowe.com/)                                                                                                                                                                            |
| **Repository**  | [Processing-language-server-extension](https://github.com/diyaayay/processing-language-server-extension)                                                                                                                                            |


## Description

This report details my work, development processes, and insights gained during the pr05 2024 "New Beginnings" software development grant with the [Processing Foundation](https://processingfoundation.org/). During the past four months, I have focussed on developing the [Processing Language Server Extension](https://github.com/diyaayay/processing-language-server-extension/) for the Processing software for VSCode. Currently, Processing is distributed as a standalone software compatible with multiple platforms; this extension integrates `.pde` sketch files into VSCode, providing robust IntelliSense support to enhance the coding experience.
It was a privilege to work under the mentorship of Sam Lavigne and advisor Justin Gitlin. Together, we conducted bi-weekly project check-ins to review progress and address challenges in development.
The Language Server Extension includes a server-side implementation and a client interface within the VSCode editor, delivering IntelliSense functionality to developers working with .pde sketches. The complete project plan is accessible within the project's [roadmap](https://github.com/diyaayay/processing-language-server-extension/blob/Development/ROADMAP.md).


## What code got merged in
- [Run Processing Sketches](https://github.com/diyaayay/processing-language-server-extension/blob/Development/client/src/sketchRunner.ts): 
The Run and Stop button toggle according to the state of the sketch. The sketchbook can contain multiple sketches each residing in a directory of the same name as that of the sketch.
- [Grammar](https://github.com/diyaayay/processing-language-server-extension/tree/Development/server/src/grammar/terms): Defines Processing specific grammar model to parse the textDocument in workspace.
- [Parser](https://github.com/diyaayay/processing-language-server-extension/blob/Development/server/src/parser.ts): The parser.ts file defines functions to parse Java code into an Abstract Syntax Tree (AST) and extract tokenized information from the code. The main function, parseAST, processes Java code and returns an array of token pairs representing each parse tree node and its parent. The lineMap function creates mappings of each word in a line to its start and end positions, helping locate tokens within specific lines. The parse function leverages the java-ast library to handle parsing while muting unnecessary console output through a redirectConsole utility, which temporarily disables logging during the parsing process.
- [Sketch Runner](https://github.com/diyaayay/processing-language-server-extension/blob/Development/client/src/sketchRunner.ts): Defines a SketchRunner class that manages the execution of Processing sketches in a Visual Studio Code extension. It provides functionality to initialize the sketch runner with a specified Java Runtime Environment (JRE) path and a sketch directory. The class can run and stop sketches while handling output from the Processing environment through buffered channels to manage the display in the VSCode output window. It also includes error logging to capture issues during execution and during the copying of compiled sketches. The copyCompiledSketch method ensures that .class files are correctly transferred from the source to the destination directory, maintaining the structure of the original sketch. Additionally, the dispose method cleans up resources when the sketch runner is no longer needed, preventing memory leaks. Overall, the class encapsulates the functionality required to run and manage Processing sketches effectively within the editor environment.
- [Sketch Errors](https://github.com/diyaayay/processing-language-server-extension/blob/Development/client/src/sketchRunner.ts): On Running a sketch, the LSP extension starts watching the sketch and logs errors in the Output tab.
- [On-Hover Documentation](https://github.com/diyaayay/processing-language-server-extension/blob/Development/server/src/hover.ts): The on-hover feature would display the documentation for every keyword of the Processing language stored in [XML files](https://github.com/diyaayay/processing-language-server-extension/tree/Development/server/out/processing/lspinsights).
- [Code Completion](https://github.com/diyaayay/processing-language-server-extension/blob/Development/server/src/completion.ts): Keywords are reserved keywords in the Processing language. This feature would display a pop-up menu containing a list of keywords aligning with the triggering character.
- [Goto Reference](https://github.com/diyaayay/processing-language-server-extension/blob/Development/server/src/references.ts): This feature responds to the onReferences requests by the user when they select the option of, found in the context menu to navigate to the schemas that reference the location under the cursor.
- [Goto Definition](https://github.com/diyaayay/processing-language-server-extension/blob/Development/server/src/definition.ts): This is vice-versa of the former issue i.e., jump to the schema a reference refers to. This is the same as going to the variable declaration through `Ctrl/Cmd + Click` or the context menu. This uses the `onDefinition` request from the user in the extension host to redirect the cursor to the definition the reference refers to.

## Techincal challenges and decisions taken:

- **Running Processing headless** without requiring the user to set paths or require some complex setup was the main challenge to make the version migration work everywhere.
  - This required quite a bit of research and decisions taken regarding bundling everything in the extension:- We aimed to run the processing sketches inside the extension host (which is different from running the sketches in a JavaScript frontend, for example). We could simply call the binary using some `npm` package like `child_process`, `spawn-process`, etc., but then we need it inside the extension host as it should also have the client-server support - necessary to make all LSP features work smoothly.
  - For example, [vscode-languageserver-node](https://github.com/microsoft/vscode-languageserver-node) is using a custom script called `symlink` which can be executed normally as `npm run symlink`. It just executes a file called [symlink.js](https://github.com/microsoft/vscode-languageserver-node/blob/main/build/bin/symlink.js) which creates the hardlinks and softlinks between the protocol, the server, the client and the extension folder. Some languages have better low-level control, like [C](https://pubs.opengroup.org/onlinepubs/009696899/functions/symlink.html#:~:text=The%20symlink()%20function%20shall,contained%20in%20the%20symbolic%20link) and [Rust](https://doc.rust-lang.org/stable/std/os/unix/fs/fn.symlink.html) that support links creation.
  - Opting for a similar approach, the Processing distribution is binded to the extension. The large size of the extension is due to custom dependencies downloaded via the [dependency.bat](https://github.com/diyaayay/processing-language-server-extension/blob/Development/dependency.bat) script. The [dependency.zip](https://github.com/diyaayay/processing-language-server-extension/releases/download/v1.2/dependency.zip) contains the Processing distribution for Windows, which is extracted into `jre/deps`.
- **Release binding with commit**: I encountered an unexpected issue with GitHub releases that took me some time to understand. When the maintainer creates a GitHub release, it automatically binds to a commit on the default branch. When this branch is cloned, it causes multiple issues, including connection failures with the LSP client and errors related to the VSCode API. After some research, I resolved the problem by binding GitHub releases to a commit on a branch other than the default branch.

## Current state of the Project:

As outlined in the project roadmap, the LSP extension has been developed for cross-platform compatibility. Packaging Processing's latest version to bundle with the extension and run from its server was achieved by creating symlinks using Rust (a low-level language) for **Windows**. This eliminated the need for users to add the Processing path to their environment variable and enabled Processing to run in headless mode. This method in the future would enable the distribution to run directly from the extension on all platforms. The following milestones have been successfully achieved:

- [x] Stable Architecture
- [x] .vsix release and VSCode Marketplace release
- [x] Integrate Processing 4.3
- [x] Workspace management
- [x] Run and Stop Buttons
- [x] On-hover documentation
- [x] Syntax highlights
- [x] Sketch Errors
- [x] Run Processing sketches headless
- [x] Run Multiple Sketches
- [x] Workspace Management (Sketch and Sketchbook)
- [x] Go-to-definitions
- [x] Go-to-References
- [x] Code Completion


## Limitations
- The LSP features work cross-platform, but sketch execution is currently limited to **Windows** due to the specific [symlink](https://doc.rust-lang.org/std/os/unix/fs/fn.symlink.html) setup for the Windows distribution.
- The large size of the extension is due to custom dependencies downloaded via the `dependency.bat` script. The [dependency.zip](https://github.com/diyaayay/processing-language-server-extension/releases/download/v1.2/dependency.zip) contains the Processing distribution for Windows, which is extracted into `jre/deps`.
- This limitation is due to the extension's JSON-RPC architecture, which enables client-server communication for IntelliSense features. A layered structure was established to connect the protocol, server, client, extension, and Processing distribution, using symlinks to package Processing and execute sketches from the sketch runner.
- Future development could use similar setups for other platforms to expand compatibility.
  
## Future Goals
The following objectives are prioritized for future contributions to the project:
- Establish a comprehensive testing strategy with full feature tests.
- Extend Processing's AST and AST-supported features.
- Bundling of Processing distribution by creating symlinks for multiple platforms.

While VSCode remains one of the most widely used code editors, extending support for this LSP to other editors, including JetBrains, Eclipse, and CodeMirror, can also be a future goal.

## Relevant Commits and Issues
- All feature commits are here: [merged code](#what-code-got-merged-in)
- [LSP Release for Processing 4 Beyond the Current Workaround](https://github.com/diyaayay/processing-language-server-extension/issues/1)
- [Licensing discussion](https://github.com/diyaayay/processing-language-server-extension/issues/1)
- [Dynamic sketchpaths](https://github.com/diyaayay/processing-language-server-extension/commit/80e13ed678c8eb464fcccd9b2136fbbd543c25ba)

## Important Links
- [Project Link](https://github.com/diyaayay/processing-language-server-extension/)
- [Monthly Updates](https://github.com/processing/pr05-grant/tree/main/2024_NewBeginnings/monthly-reports)
- Marketplace Release: [Download](https://marketplace.visualstudio.com/items?itemName=DiyaSolanki.processing-language-server-extension)
- [GitHub release](https://github.com/diyaayay/processing-language-server-extension/releases/tag/v0.0.1)
- Dependency release: [Processing bundle](https://github.com/diyaayay/processing-language-server-extension/releases/tag/v1.2)