# Chap 6 | Side Effects

## Side Effects

As we build applications, we often need to synchronize with external systems. This can include things like:

- Making network requests
- Managing timeouts / intervals
- Reading/writing from localStorage

- Listening for global events

React calls all of these things “side effects”.

## The useEffect Hook

- Core React loop ➡️ We give React a chunk of JSX and it produces the resulting DOM .When the JSX changes (eg because the state or props change), React will update the DOM to match, always keeping it in sync .This is React's main job , its primary purpose
- Things that fall outside of this main job like updating the document's title , making network requests , interacting with local storage are known as side effects 
- React gives us a specific tool for managing side effects : the useEffect hook .We pass some code to `useEffect` , via a callback function , and React calls that function for us after each render
- **Why not do the work directly in the component?** Well, we *can*, but `useEffect` gives us additional tools.
- We can specify *dependencies* so that the effect only runs sometimes, when a particular value has changed. For example:

```js  
React.useEffect(() => {
  // Effect logic
  document.title = `(${count}) — Counter 2.0`;
}, [count]);
```

- By specifying `[count]`, we're saying that the effect logic should *only* run when the `count` variable changes.
- Effects always run after the very first render, no matter what we specify in our dependency array.
