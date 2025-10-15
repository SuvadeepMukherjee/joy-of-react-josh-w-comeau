# Chap 3 | The useState Hook

## Minimal Counter App with useState Hook

```jsx
import React from 'react';

function Counter() {
  const [count, setCount] = React.useState(0);

  return (
    <button onClick={() => setCount(count + 1)}>
      Value: {count}
    </button>
  );
}

export default Counter;
```

Our goal is to keep track of the number of times the user has clicked the button. Whenever we have “dynamic” values like this, we need to use React state. State is used for values that change over time.

To create a state variable, we use the `useState` function. This function takes a single argument: the initial value. In this case, that value initializes to `0`. This value is chosen because when the page first loads, we've clicked the button 0 times.

`useState` is a *hook*. A hook is a special type of function that allows us to "hook into" 
React internals. 

The `useState` hook returns an array containing two items:

1. The current value of the state variable. We've decided to call it `count`.

2. A function we can use to update the state variable. We named it `setCount`.

## Initial State

React state variables can be given an initial value:

```jsx
const [count, setCount] = React.useState(1);
console.log(count); // 1
```

We can also supply a *function*. React will call this function on the very first render to calculate the initial value:

```jsx
const [count, setCount] = React.useState(() => {
  return 1 + 1;
});

console.log(count); // 2
```

This is sometimes called an *initializer function*.It can occasionally be useful if we need to do an expensive operation to calculate the initial value. For example, reading from Local Storage:

```jsx
const [count, setCount] = React.useState(() => {
  return window.localStorage.getItem('saved-count');
});
```

If you're not familiar with the Local Storage API, it's a way for us to save values on the user's device, so that it persists even after the browser tab is closed, and can be accessed on their next visit.

The benefit here is that we're only doing the expensive work (reading from 
Local Storage) once, on the initial render, rather than doing it on every single render.

## What exactly is the difference between these 2 forms ?

```jsx
// Form 1:
const [count, setCount] = React.useState(
  window.localStorage.getItem('saved-count')
);

// Form 2:
const [count, setCount] = React.useState(() => {
  return window.localStorage.getItem('saved-count');
});
```

Let's say we have this function:

```js
function run() {
  console.log('Hello');
}
```

Whenever I call the `run` function, we'll log the word "Hello". If I call the function 5 times, I'd get 5 "Hello"s in the console.

Now, suppose we tweak it:

```js
function run() {
  const sayHi = () => {
    console.log('Hello');
  };
}
```

Now, when I call the `run` function, nothing is logged to the console. That's because the `console.log` is packaged up in an inner function, `sayHi`. Every time we call the `run` function, we're *creating* a brand new `sayHi` function, but we're never *calling* it.

Alright, now let's look at the React code again:

```js
function Counter() {
  const [count, setCount] = React.useState(
    window.localStorage.getItem('saved-count')
  );
}
```

Whenever we render the `Counter` component, this function will be called, and all of the code inside will run. We'll call `window.localStorage` on every single render, and pass the value into `React.useState`.

Now, let's consider the secondary form:

```jsx
function Counter() {
  const [count, setCount] = React.useState(() => {
    return window.localStorage.getItem('saved-count');
  });
}
```

In this case, we're *creating a function*:

```jsx
() => {
  return window.localStorage.getItem('saved-count');
}
```

This *function* is being passed into `React.useState()`. And so it's up to React to decide what to do with it.

On the very first render, React will call this function to calculate the initial value. On *subsequent* renders, however, React ignores the function. The initial value has 
already been calculated, and so there's no reason to call the function 
again.

---
