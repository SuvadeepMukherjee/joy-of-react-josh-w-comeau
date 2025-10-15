# Chap 3 | The useState Hook

Anki => 03-useState-hook

**Q1**:In React,how do you declare a state variable named `count` initialized to `0` and display it inside a button that increments the value by 1 each time it's clicked ? 

**Solution**:

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

**Q2**:What is initializer function in useState and why it is useful ? 

**Solution**:

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

The benefit here is that we're only doing the expensive work (reading from 
Local Storage) once, on the initial render, rather than doing it on every single render.

**Q3**:What exactly is the difference between these 2 forms ? 

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

**Solution:**

```jsx
function Counter() {
  const [count, setCount] = React.useState(() => {
    return window.localStorage.getItem('saved-count');
  });
}
```

Whenever we render the `Counter` component, this function will be called, and all of the code inside will run. We'll call `window.localStorage` on every single render, and pass the value into `React.useState`

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
