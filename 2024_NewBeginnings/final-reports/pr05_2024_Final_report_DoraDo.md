# Prototype a Collaborative Editor for Processing - Final Report
## Technical Decisions
Throughout this project, several technical choices were crucial in achieving a stable, cross-platform application. Here are the primary decisions:

- UI Design & Development: 
  - React & Zustand: React was chosen for its component-based architecture and Zustand for state management. This combination provided a scalable and maintainable structure for the UI.
  - Radix UI Component Library: Integrated Radix UI components to enhance the app's visual design and user experience. This library provided a consistent design system for future contributors and improved the overall UI.
  - Codemirror: Originally, we considered Theia, but opted for CodeMirror due to its ease of implementation, extensive use in the p5 Web Editor, existing collaborative features, and faster development potential. This choice was validated by the successful integration of CodeMirror with Electron.

- Real-time Collaboration:
  - CodeMirror & YJS Integration: We successfully integrated the YJS framework with the CodeMirror editor to enable real-time collaboration. This integration was essential for the core functionality of the app and laid the foundation for collaborative editing. We used the `y-codemirror.next` package with the `y-websocket` provider for real-time synchronization of CodeMirror documents.
  - Websockets: We used `y-websocket` for the WebSocket provider to enable real-time collaboration. This choice was made after evaluating WebRTC's limitations in handling larger groups effectively due to performance issues. We decided that Websockets were more stable for our use case and provided better support for scaling. Currently, the server is hosted on a cloud service called Render on their free-tier plan, which puts the server to sleep after a period of inactivity. The repository to the server can be found [here](https://github.com/doradocodes/pce-server). When a user hosts a collaborative session, a room is created on the server with a unique ID (the Room ID). This ID is then shared to the other users who want to join the session. Since the implementation was a bit crude for this prototype, rooms are not deleted after the session ends. But if a user drops from a session, they can still rejoin the session using the same Room ID, even if the host disconnects.

- Running Processing Sketches: 
  - Integrating Processing sketches was foundational to the app’s core functionality. In this prototype, sketches are saved locally as `.pde` files and  Electron runs the `processing-java` executable to execute Processing sketches with a path to the file. It is not the most efficient method of running sketches, but it allowed us to demonstrate the collaborative editing of Processing code. Future iterations will include a headless version of Processing for more efficient sketch execution.
  
- App Build and Distribution: 
  - We initially tested app distribution using electron-builder, but challenges in handling Apple’s certificate verification led to adopting electron-forge, which streamlined the macOS notarization process. electron-forge provided more reliable handling for platform-specific requirements, making it easier to sign and notarize the app for Mac and set up for Windows. There was also an issue with notarizing the Processing library, which was resolved by creating a script called `sign_jnilib_files.js` to sign the library for notarization.
  - In order for others to build the app, contributors can use the provided `.env.example` file to set up their environment variables. 


## Tasks Completed
- Created a functional prototype of a collaborative code editor for Processing using Electron and CodeMirror.
- Allow creation, editing, renaming, deleting, and running of local Processing sketches.
- Integrated YJS with CodeMirror for real-time collaboration, enabling multiple users to edit a Processing sketch simultaneously.
- Allows user selection of dark or light mode for the editor.
- Builds for macOS and Windows.


## Challenges
- Certificate and Notarization Issues: Encountering "damaged app" errors when testing macOS builds on other machines highlighted the need for proper app signing. After troubleshooting with Apple Developer tools and certificates, switching to electron-forge resolved this issue.

- Cross-Platform Compatibility: Distribution on Windows and Linux introduced different runtime and dependency challenges. Upcoming tasks will focus on thoroughly testing builds on these platforms.

- UI Bug Fixes: Addressing minor yet crucial UI bugs required repeated testing. Adding collapsible panels and submenus also introduced layout adjustments that required careful tuning to avoid disrupting the user flow.


## Limitations
- Apple Developer Account Dependency: Open-source contributors must have Apple Developer credentials for macOS builds, which may limit participation without a centralized solution.
- Websocket server: A crude websocket server was used for real-time collaboration, which may not be scalable for larger groups. Future iterations will explore more robust solutions for handling collaborative editing.
- Processing Java library and JDK storage: For this project, the Processing Java library, nor the JDKm were committed to the repository due to its size. Contributors must download the Processing4 application, go into it's Contents folder and copy the Java folder into the tools/PlugIns folder. This is a temporary solution until a more automated process is implemented.
- Windows build: The Windows build has not been fully developed or tested, since my development environment is macOS. Future work will focus on ensuring the app runs smoothly on Windows systems.
- Single Tab View: The current version only supports a single tab for editing sketches since it was out of scope for the timeline of this prototype.
- Separate Collaboration Window: In order to keep the Websocket connection straightforward, when the user starts a collaborative session, a new window is opened with the collaborative session. This was a solution to make connection/disconnection to specific rooms easier, since connection to a room is by URL. A future iteration should maintain rooms via Websocket messages so that the user could have multiple collaborative sessions open at once on the same window.

## Future Vision & Improvements
- Running sketches with a headless Processing Library: This will streamline the execution of Processing sketches within the app, improving performance and user experience.
- LSP Integration: Implementing the Processing Language Server Protocol will enhance coding features, such as autocompletion and syntax highlighting, for a more robust editing experience.
- Allow Imports of External Processing Libraries: Enabling users to import external Processing libraries will expand the app's functionality and utility for Processing users.
- Importing Assets: Adding support for importing assets like images and sounds into the Processing sketch will enhance the creative capabilities of users.
- Peer-to-peer Collaboration: Exploring peer-to-peer connections with WebRTC for real-time collaboration will provide an alternative to WebSocket-based collaboration, offering more flexibility and scalability.
