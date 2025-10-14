# Chap 2 | Event Handlers

Anki ➡️ 02-event-handlers

**Q1**:In react how do you attach an event handler function (like doSomething) to a button so that it runs when the button is clicked ?

**Solution**:

```jsx
function App() {
  function doSomething() {
    // Stuff here
  }

  return (
    <button onClick={doSomething}>
      Click me!
    </button>
  );
}
```

**Q2**:

The below code snippet shows the recommended way of attaching an event handler function in React 

```jsx
function App() {
  function doSomething() {
    // Stuff here
  }

  return (
    <button onClick={doSomething}>
      Click me!
    </button>
  );
}
```

- When do we use `addEventListener` ? 
- Why the above code snippet is the recommened way of handling events in React ? 

**Solution**:

- We sometimes use `addEventListener` for window-level events 
- The above code snippet is the recommended way to handle events for the following reasons 
  - **Automatic cleanup** : Whenever we add an vent listener, we're also supposed to remove it when we're done, with `removeEventListener`. If we forget to do this, we'll introduce a memory leak.(Memory leaks happen when our  application fails to “release” something from memory when it should. As a result, the application continues to eat up more and more memory, until eventually the browser tab crashes) React automatically removes listeners for us when we use “on X” handler functions.
  - **Improved performance.** By giving React control over the  event listeners, it can optimize things for us, like batching multiple  event listeners together to reduce memory consumption.
  - **No DOM interaction.** React likes for us to stay within its abstraction. We generally try and avoid interacting with the DOM directly.

**Q3**:What is the difference between React's way of handling events and HTML way of handling events ? 

**Solution**:

**camelCasing**:

we need to write attribute names in camelCasing

**Passing a function reference**:

When working with event handlers in React, we need to pass a *reference* to the function. We don't call the function, like we do in HTML:

```jsx
// ✅ We want to do this:
<button onClick={doSomething} />

// 🚫 Not this:
<button onClick={doSomething()} />
```

**Q4**:When working with event handlers in React , we need to pass a reference to the function.We dont call the function like we do in HTML 

HTML way of handling event handlers ➡️

```html
<button onclick="doSomething()">
  Click me!
</button>
```

React way of handling event handlers ➡️

```jsx
<button onClick={doSomething}>
      Click me!
 </button>
```

What is the reason behind this ? 

**Solution**:

When we include the parentheses, we invoke the function right away, the  moment the React application is rendered. We don't want to do that; we  want to give React a *reference* to the function, so that React can call the function at a later time, when the user clicks on the button.

```jsx
// ✅ Correct.
// Will be called when the user clicks the button.
React.createElement(
  'button',
  {
    onClick: doSomething,
  }
);

// 🚫 Incorrect
// Will be called immediately, when the element is created.
React.createElement(
  'button',
  {
    onClick: doSomething(),
  }
);
```

## 

**Q5**:In React how do you attach an event handler function like setTheme with an argument of say 'black' to a button such that it runs when the button is clicked ? 

**Solution**:

```jsx
<button onClick={() => setTheme('dark')}>
  Toggle theme
</button>
```

**Q6**: In React, if we want to call a function like `setTheme('dark')` when a button is clicked, we usually pass an anonymous function like this:

```jsx
<button onClick={() => setTheme('dark')}>
  Toggle theme
</button>
```

Is passing an anonymous function directly to `onClick` bad for performance?

**Solution**:

- There isn't a measurable performance difference between creating anonymous  functions vs. named functions. Same thing for arrow vs. traditional  functions.

- The cost of creating a new function is extremely low. Even low-end devices can create *hundreds of thousands* of functions in the span of a human blink.

- React has a built-in event delegation system that implements a bunch of optimizations for us.