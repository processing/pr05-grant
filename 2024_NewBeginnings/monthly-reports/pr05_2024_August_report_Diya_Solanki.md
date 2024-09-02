# August Monthly Report: Develop a VSCode LSP Server Extension for Processing
By **Diya Solanki**

- [Project Repository: Processing Language Server Extension](https://github.com/diyaayay/processing-language-server-extension/)
- [Working Branch: main](https://github.com/diyaayay/processing-language-server-extension/blob/main)

## Completed Tasks (what's done so far)

- [x] [Using the VSCode APIs](https://code.visualstudio.com/api/language-extensions/language-server-extension-guide) to set up a robust client-server architecture for establishing the connection between the server and the extension-host.
- [x] Setup an `extension.ts` file which handles the spawning of the processing process, has the integrated `Run and Stop` buttons, processing commands for VSCode launch tasks, created a buffered channel, etc.
- [x] [Resolved path](https://github.com/diyaayay/processing-language-server-extension/commit/dd887c14ef74cdba875abd59ecd210caf49b7c59) using `vscode-uri` which was earlier `filePathToURI` from `node:uri`.
- [x] Researched into establishing an abstract syntax tree for processing language syntax; still wondering over ANTLR, lexer and parser; or writing a static AST.
- [x] [Added Java Grammar support:](https://github.com/diyaayay/processing-language-server-extension/commit/7c91e7a99172740fda46ff796ea35af1a713cb55) Integrated the top level Java keywords like `private`, `static`; class body keywords like `extends`; method body keywords like `syscalls(), trim()` etc.
- [x] [Added standard keyword support for processing](https://github.com/diyaayay/processing-language-server-extension/commit/74eb1cb54fef02381c3be6542b73b6c665425f3a) which includes the code snippets for Java processing.
- [x] Analysed a Processing [VSCode Extension](https://github.com/avinZarlez/processing-vscode) by Avin to see the approach of running processing sketches inside the extension host. `Conclusion as of now`: We are using the similar approach as of now, i.e., to use the `child_process` library to spawn a processing process; this is a work in progress.
- [x] `Solved`: Other features such as `Run & Stop` buttons (for running and stopping sketches from rendering) in the extension host for ease of use has been integrated in the client-side (can be found in the extension host). 


## Challenges Encountered

- Currently, I have created some [symlinks](https://www.linode.com/docs/guides/linux-symlinks) which : a) points to some [inodes](https://docs.rackspace.com/docs/what-are-inodes-in-linux), and b) references to some of the dependencies that are required to run the processing binary. So, as of now, the sketch renders (inside the extension host) but it requires some setup. The main challenge is to make the version migration work everywhere - which I am currently working on.
- We aim to run the processing sketches inside the extension host (which is different from running the sketches in a JavaScript frontend, for example). We could simply call the binary using some `npm` package like `child_process`, `spawn-process`, etc., but then we need it inside the extension host as it should also have the client-server support - necessary to make all LSP features work smoothly.
- I am reading a bit more on symlinks and exploring what all options do we have for the wrappers. For example, [vscode-languageserver-node](https://github.com/microsoft/vscode-languageserver-node) is using a custom script called `symlink` which can be executed normally as `npm run symlink`. It just executes a file called [symlink.js](https://github.com/microsoft/vscode-languageserver-node/blob/main/build/bin/symlink.js) which creates the hardlinks and softlinks between the protocol, the server, the client and the extension folder. Some languages have better low-level control, like [C](https://pubs.opengroup.org/onlinepubs/009696899/functions/symlink.html#:~:text=The%20symlink()%20function%20shall,contained%20in%20the%20symbolic%20link) and [Rust](https://doc.rust-lang.org/stable/std/os/unix/fs/fn.symlink.html), if that can help better with the system level symlinks; I am exploring these options also.
- After this is done, it will be ready for testing and releasing, ASAP.

## Plans for September

- [ ] `Goal`: Two features per week:
    - [ ] Code-completion
    - [ ] Diagonostics
    - [ ] Hover
    - [ ] Syntax highlighting
    - [ ] `onReferences`
    - [ ] `onDefinition`
    - [ ] `...`
- [ ] `...`