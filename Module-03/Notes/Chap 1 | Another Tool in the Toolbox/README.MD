# Chap 1 | Another Tool in the Toolbox

## React Hooks

Earlier components were the only tool in the toolbox. Every problem in React was solved with components.We used components for all kinds of things including:

- Pulling data from context

- Network Requests

- Behaviours like scrolling to the top of the page on mount

- Lazy-loading other components

All of these non-UI usecases led to a common scenario called *Wrapper Hell*:

In order to help with these issues, the React team developed a *second* tool for the toolbox: **hooks.**

The really cool thing about hooks is that they allow us to package up and re-use business logic.Like how a `Button` component can be sprinkled around our application, we can create custom bundles of business logic, like `useMousePosition` or `useSpringAnimation`, which can be shared between components.
