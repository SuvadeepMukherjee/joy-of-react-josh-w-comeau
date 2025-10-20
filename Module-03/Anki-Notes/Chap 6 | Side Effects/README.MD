# Chap 6 | Side Effects

Anki ➡️ 06-side-effects

**Q1**:What are Side Effects ? 

**Solution**:

- **Core React Loop** ➡️ We give React a chunk of JSX and it produces the resulting DOM .When the JSX changes (eg because the state or props change), React will update the DOM to match, always keeping it in sync .This is React's main job , its primary purpose
- Things that fall outside of this main job like *updating the document's title* , *making network requests* , *interacting with local storage* are known as side effects 

**Q2**:Why React team gave us the `useEffect` hook ? 

**Solution**:

- Core React loop ➡️ We give React a chunk of JSX and it produces the resulting DOM .When the JSX changes (eg because the state or props change), React will update the DOM to match, always keeping it in sync .This is React's main job , its primary purpose
- Things that fall outside of this main job like updating the document's title , making network requests , interacting with local storage are known as side effects 
- React gives us a specific tool for managing side effects : the useEffect hook

**Q3**:Explain the useEffect Hook ? 

**Solution**:

**No dependency array** ➡️

`useEffect(() => { … })`

- Runs after every render , both initial and subsequent renders

**Empty dependency array**

`useEffect(() => { … }, [])`

- Runs only once , after the first render (component mount)

**Dependency array with variables**

`useEffect(() => { … }, [var1, var2])`

- Runs **after the first render**
- And then **any time any of the variables in the array change**.

**Q4**:Suppose we have the following code:

```jsx
function App() {
  const [count, setCount] = React.useState(0);

  React.useEffect(() => {
    console.log(count);
  }, []);

  return (
    <>
      <p>The count is: {count}</p>
      <button onClick={() => setCount(count + 1)}>
        Increment
      </button>
    </>
  );
}
```

-  If we run this code what error will we get ? 
- How to disable the error ? 
- Why do we get this error ? 
- How to fix the error ? 

**Solution**:

**The error that we will get** ➡️

![lint-warning](../../assets/lint-warning.png)

**To disable the error we can do the following** ➡️

To disable the warning(we are not fixing the error) we can do the following : 

```jsx
React.useEffect(() => {
  console.log(count);
  // eslint-disable-next-line
}, []);
```

This magical incantation will silence the warning, but it doesn't solve the problem!

**Why do we get this error ? **

This rule isn't arbitrary, it's here for a very good reason: if we don't include the `count` state variable in the dependency array, we'll wind up in an *extremely* confusing situation.

Every time we re-render, by calling `setCount`, we take a new snapshot, and create a new `count` variable. This means that there are different "versions" of the `count` variable, one for each snapshot.

By not including `count` in the dependency array, it means that our effect is "frozen in time"; the `console.log(count)` code inside the effect only has access to the *first* `count` variable, the one equal to `0`.

It leads to this bewildering situation, when the state changes:

```jsx
function App() {
  // This `count` is equal to 3:
  const [count, setCount] = React.useState(0);

  React.useEffect(() => {
  // This `count` is equal to 0:
    console.log(count);
  }, []);

  return (
    <>
      {/* this `count` is equal to 3: */}
      <p>The count is: {count}</p>
      <button onClick={() => setCount(count + 1)}>
        Increment
      </button>
    </>
  );
}
```

Our effect has gotten out-of-sync with the rest of the code, and it  means that we're seeing multiple versions of the state at the same time.

**How to fix the error ? **

To fix it, we need to add `count` to the dependency array:

```jsx
React.useEffect(() => {
  console.log(count);
}, [count]);
```

This means that the effect will re-run whenever we call `setCount`, keeping it in sync as the state changes.
