# September monthly report: Prototyping a collaborative code editor for Processing
By Dora Do

## Project Overview

This month focused on refining the collaborative editing functionality of our Electron-based code editor, designed for Processing sketches. We explored different real-time collaboration frameworks, worked on packaging the app for distribution, and continued improving the user interface. Notable progress was made in integrating YJS with CodeMirror for real-time collaboration and addressing the challenges of running processing-java within the app across different operating systems.

## Key Decisions and Progress

We decided to use WebRTC for small groups (1-3 users) but identified its limitations in larger groups due to performance issues. Therefore, we are leaning towards WebSockets for text-based messaging due to their reliability and better support for scaling. We successfully integrated the YJS framework with the CodeMirror editor and tested it with WebSocket providers. The Radix UI component library was also integrated into the app to enhance the overall user interface and provide a consistent design system for future contributors.

On the packaging front, we used `electron-builder` to package the app and have it running from the Applications folder. Some issues with paths required adjustments to the `package.json` and asset paths, but this was resolved, allowing for successful packaging.

## Challenges and Solutions

- Scaling WebRTC: 
  - We realized WebRTC could not handle larger groups effectively due to performance issues. We are now focused on WebSockets, which are more stable for our use case.
- Handling initial state sync in collaborative sessions: 
  - We faced a problem where new clients joining a collaborative sketch were not receiving the initial document state. The solution involved setting the entire document upon room creation, allowing new clients to receive the correct state.
- Running Processing sketches in the packaged app: 
  - Running the `processing-java` executable in the packaged app posed issues due to hardcoded paths for the Java library and JDK. The solution we implemented was a Settings popup where users can manually select the path to their Processing installation. This is a temporary workaround until more automated solutions, such as Stef's headless version of Processing, are available
- Error handling when clients join non-existent sketches:
  - There was an issue with properly handling errors when clients try to join a sketch that doesn't exist. We're currently improving error handling to detect and notify users when they attempt to join an invalid sketch.

## Next Steps

1. **Further Testing**: We need to conduct more extensive tests on both macOS and Windows, especially ensuring that the `processing-java` executable works across platforms.
2. **Improve User Onboarding**: I plan to implement an onboarding flow that will guide users through setting up their Processing environment. This will include selecting their Processing installation path and verifying that their system is ready to run sketches.
3. **Enhance UI and UX**: Based on Ted’s feedback, we redesigned the side navigation and improved the sketch creation workflow. Further refinement is needed for a more intuitive user experience.
4. **Fixing Known Bugs**: Continue to refine error handling for clients joining invalid rooms and improve document state synchronization in collaborative sessions.

## Conclusion

This month saw significant progress in real-time collaboration, UI refinement, and resolving packaging issues for app distribution. While there are still challenges, especially around running Processing sketches and scaling WebRTC, we've implemented viable solutions for the next phase of testing. With more rigorous testing planned, we're on track to deliver a stable prototype for broader feedback and iteration.
