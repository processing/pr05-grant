# August monthly report: Prototyping a collaborative code editor for Processing
By Dora Do

## Project Overview

This month, we made significant progress on the Processing Collaborative Editor (PCE) project. Our focus was on evaluating technology options, designing the user interface, and beginning the implementation of core features.

## Key Decisions and Progress

- Technology Stack:
    - After careful consideration and discussions with mentors, we decided to proceed with a custom build using CodeMirror rather than Theia.
    - This decision was based on CodeMirror's ease of implementation, existing collaborative features, and faster development potential.
- User Interface Design:
    - Created wireframes and a Figma prototype for the PCE UI.
    - The design features a two-column layout with collapsible sections for improved usability.
- Development Progress:
    - Successfully integrated CodeMirror editor with Electron.
    - Implemented basic functionality to run Processing sketches from within the application.
    - Began integration of CodeMirror's collaborative features.

## Challenges and Solutions

We encountered some technical challenges, particularly with running the `processing-java` process when using Vite. This was resolved by adjusting our build process to use Vite only for the React app portion.

## Collaborative Features

We're exploring two main approaches for implementing real-time collaboration:

1. Using a locally hosted intermediary server with web sockets
2. Using peer-to-peer connections with WebRTC

Each approach has its pros and cons, and we're currently evaluating which will be the best fit for our project goals.

## Next Steps

- Complete the integration of CodeMirror's collaborative features
- Refine the UI design and implement remaining layout components
- Decide on and implement the collaborative connection approach
- Begin work on integrating Language Server Protocol (LSP) for enhanced coding features

## Conclusion

August has been a productive month for the PCE project. We've made critical decisions on our technology stack, advanced our UI design, and begun implementing core features. As we move into September, our focus will be on refining the collaborative aspects of the editor and continuing to improve the overall user experience.
