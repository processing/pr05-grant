Monthly Report - August 2024
Overview: This month, I focused on resolving issues related to the useEffect dependency array and researched ways to improve the integration of @uiw/react-codemirror with React lifecycle events, particularly in the context of CodeMirror 6. Here’s a detailed breakdown of progress:

1. Resolving useEffect Dependency Array Issues:

Identifying the Issue:
I identified that most of the issues were stemming from the way the dependency array in useEffect was structured. This required further investigation to determine which variables could safely be removed from the array without affecting functionality.

Proposed Solutions:

Connie’s Idea: Connie suggested separating useEffect hooks as much as possible, so each one has its own relevant dependency array. This would minimize unnecessary re-renders and make the code cleaner.
Additionally, Connie proposed that leaving the dependency array empty in cases where the useEffect acts like componentDidMount seemed to alleviate some issues, particularly around tidyCode. This needs further testing to confirm stability.
2. Researching and Testing @uiw/react-codemirror:

Goal:
My goal was to explore how to better incorporate React’s lifecycle with CodeMirror 6, improving overall integration and reducing potential issues.

Current Progress:

I worked on converting the Editor/index.jsx file using @uiw/react-codemirror and tested this integration extensively. A branch/PR has been created for tracking the changes:
PR link

Key Changes Implemented:

Switched to using EditorView along with a editorView state in React instead of relying on cmInstance.current.
Adopted view.dispatch for applying changes to the editor view, replacing the previous method of calling functions directly from the instance.
Modified the usage of linter and Emmet to align with CodeMirror 6 updates.
Next Steps:
I plan to create a custom hook (useCodemirror) to streamline the setup process for the CodeMirror view, which will make the code more maintainable and reusable.

3. React Functional Component Conversion:

Changes Applied:
I completed the React functional component conversion for the following components: CollectionList, SketchList, FileNode, ConsoleInput, and AssetList. This was the last conversion ticket in the project, marking the completion of this task.

Looking Forward: In the coming month, I will continue refining the use of the @uiw/react-codemirror library and conduct additional testing to ensure smooth integration. I will also explore further improvements to the useEffect implementation to optimize performance and stability in the application.
