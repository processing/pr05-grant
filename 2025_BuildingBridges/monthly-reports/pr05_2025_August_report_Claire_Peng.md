# Monthly Report - August: Incremental Typescript Migration for the p5.js Web Editor

## Overview

Over August, the PRs from July were reviewed, and received really useful feedback to action. As such I spent a great portion of the month going back to old PRs, updating to address feedback, and updating new PRs to use the learnings from these previous PRs. The learnings will be detailed below.

In addition to the refining the PRs from July, I have additionally opened PRs to:

- [Update the README to introduce the migration](https://github.com/processing/p5.js-web-editor/pull/3581)
- [Migrate the entire client/components folder](https://github.com/processing/p5.js-web-editor/pull/3619)
- [Migrate the root files related to redux, and migrate an instance of a redux reducer system (preferences system) -- draft PR](https://github.com/processing/p5.js-web-editor/pull/3633)

As of the end of August, the following have officially been merged into the codebase:

<img src="./pr05_2025_August_report_Claire_Peng_closedPRs.png">

## Key Feedback & Typescript Styling Decisions:

### Use `interface` instead of `type` for defining props:

```tsx
// ✅ interface
interface ButtonProps {
  label: string
  disabled?: boolean
  onClick: () => void
}
const Button = ({ label, disabled, onClick }: ButtonProps) => (...)

// ❌ type
type ButtonProps = {
  label: string
  disabled?: boolean
  onClick: () => void
}

// ✅ keep type for primitives
type UserId = string;
```

<details>
<summary>Why?</summary>

- Clearer semantic meaning when describing structured objects vs. primitives

</details>

### Prefer named exports over default exports

```tsx
// ✅ named
export const Button = () => (...)
import { Button } from "./Button"

// ❌ default
export default Button
import Button from "./Button"
```

<details>
<summary>Why?</summary>

- Named exports make it easy to see what's exported and imported at a glance.

- Avoid renaming confusion and mismatched imports (e.g., import X from ... when the original name differs).
</details>

### Use `enum` instead of union types for prop variants

```tsx
// ✅ Enum for button variant
enum ButtonVariant {
  Primary = "primary",
  Secondary = "secondary",
}
interface ButtonProps {
  variant: ButtonVariant;
}

// ❌ Union type for button variant, not as preferrable.
type ButtonProps = {
  variant: "primary" | "secondary";
};
```

<details>
<summary>Why?</summary>

- Enums offer explicit, centralized definitions of allowed values.

- They prevent typo-related bugs and can be extended easily, often aligning better with team-wide or design-token systems.
</details>

## Challenges, Progress & Key Decisions:

This month was quite challenging, as I am not as experience with React and React Typescript syntax. I struggled a lot with figuring out how to resolve type-errors with `React.forwardRef` especially, which came up a lot with `useModalClose` and the components that used it. I am definitely still in the progress of learning what `refs` and `forwardRefs` are in React.

Another thing that came up was learning how to infer the base React component's props to attach to a component and how to overwrite its props with `Omit`. I tried my best to follow the logic of the existing code, and I think going forward, these explicit contracts will help future contributors understand how the components work, and also help with future refactors.

One key decision that came up upon discussion with my mentor Connie, and p5.js Web Editor project lead, Rachel, was that we would de-prioritise unit testing for time. In the previous month, I was attempting to add unit tests to each component touched, but this was quite time intensive, and we decided going forward that [...]

Throughout the process this month, I carefully checked instances of useage for each component or function up for migration, and was able to delete a number of unused modules. This was super satisfying and helped clean up the codebase a lot.

## PR's:

## Next Steps:

Going forward, I now know the time-in addressing feedback on open PR's -- I shouldn't consider these 'done' till all feedback is addressed and merged, and I should expect some time to do this as I'm still learning best practices for TS.

### September goals:

- Address any feedback for outstanding PRs (redux migration instance)
- Set up TS dependencies for the server folder
- Migrate as much as possible from the following for the server folder:
