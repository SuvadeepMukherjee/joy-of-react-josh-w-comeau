# Chap 3 |Rules of Hooks

Anki ➡️03-rules-of-hooks

**Q1**:Why do we have rules for React ? 

**Solution**:

We have rules for Hooks for the following reasons 

- Hooks are plain old JS functions
- But , when we call these functions , they "hook into" React internals . And we can catch React off-guard. It expects these hook functions to be used in very specific ways,

**Q2**:What are the 2 Rules of Hooks ?

**Solution**:

There are 2 rules of React 

- Hooks have to be called within the scope of a React application. We can't call them outside of our React components.(The only exception is that we can call hooks within “custom hooks”)
- We have to call our hooks at the **top level of the component.**("Top level" = unconditional + consistent order , not "top of the file" )

**Q3**:

The 2 Rules of Hooks are as follows 

- Hooks have to be called within the scope of a React application. We can't call them outside of our React components.(The only exception is that we can call hooks within “custom hooks”)
- We have to call our hooks at the **top level of the component.**("Top level" = unconditional + consistent order , not "top of the file" )

**Solution**:

React tracks hooks by the **order they’re called**. If you call them conditionally or inside loops, that order can change between renders — causing React to mix up state between hooks and create bugs.



*Detailed Answer* 

Let's say we're building a component with 2 pieces of state:

```js
function TextInput({ id, label, type }) {
  const [x, setX] = React.useState();
  const [y, setY] = React.useState();
}
```

We call `useState()` twice, since we have 2 different state variables.

**Here's the question:** how does React *know* which piece of state we're requesting?

Notice that we aren't giving React any sort of unique ID for each piece of state. We're not doing this:

```js
const [x, setX] = React.useState({ id: 'x' });
const [y, setY] = React.useState({ id: 'y' });
```

React uses the **order of the function calls** to figure out which state to provide to each hook.

If we render a component 100 times, it should call the exact same hooks in the exact same order. When we render a hook conditionally, we make it  possible for the hooks to change from one render to another!

This leads to *very* confusing bugs, where the `y` variable could be assigned to the `x` state.