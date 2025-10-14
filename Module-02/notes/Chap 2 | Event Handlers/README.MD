# Chap 2 | Event Handlers

## Events

As the user interacts with the page, hundreds of events are fired off in response. The browser is like an invasive private investigator, tracking every little thing you do.

When we're building dynamic web applications, these events become super important. We'll listen for these events, and use them to trigger state changes. When the user clicks the "X" button, we dismiss the prompt. When the user submits the form, we show a loading spinner.

## Javascript way of handling events

In order to respond to an event, we need to listen for it. JavaScript provides a built-in way to do this, with the `addEventListener` method:

```js
const button = document.querySelector('.btn');

function doSomething() {
  // Stuff here
}

button.addEventListener('click', doSomething);
```

In this code, we're listening for a specific event (clicks) targeting a specific element (`.btn`). We have a function which is meant to handle this event, `doSomething`. When the user clicks this particular button, our handler function will be invoked, allowing us to do something in response.

## HTML way of handling events

The web platform offers another way to do this as well. We can embed our handler right in the HTML:

```html
<button onclick="doSomething()">
  Click me!
</button>
```

## React way of handling events

React piggybacks on this pattern, allowing us to pass an event handler right in the JSX:

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

As with `addEventListener`, this code will perform the same duty: when the user clicks the button, the `doSomething`function will be called.

**This is the recommended way to handle events in React.**While we do sometimes have to use `addEventListener` for window-level events , we should try and use the “on X” props like `onClick` and `onChange` whenever possible.

There are a few good reasons why:

1. **Automatic cleanup.** Whenever we add an event listener, we're also supposed to remove it when we're done, with `removeEventListener`. If we forget to do this, we'll introduce a memory leak?. React automatically removes listeners for us when we use “on X” handler functions.
2. **Improved performance.** By giving React control over the event listeners, it can optimize  things for us, like batching multiple event listeners together to reduce memory consumption.
3. **No DOM interaction.** React likes for us to stay within its abstraction. We generally try and avoid interacting with the DOM directly. In order to use `addEventListener`, we have to look up the element with `querySelector`. *This is something we should avoid.* The “happy path” in React involves letting React do the DOM manipulation for us.

## Difference between React way of handling events and HTML way of handling events 

**CamelCasing**:

We need to write camelCased attribute names in JSX . If we forget we will get a helpful console warning letting us know about our mistakes

![camelCased-attribute](../../assets/camelCased-attribute.png)

**Passing a function reference**:

When working with event handlers in React, we need to pass a *reference* to the function. We don't call the function, like we do in HTML:

```jsx
// ✅ We want to do this:
<button onClick={doSomething} />

// 🚫 Not this:
<button onClick={doSomething()} />
```

When we include the parentheses, we invoke the function right away,  the moment the React application is rendered. We don't want to do that;  we want to give React a *reference* to the function, so that React can call the function at a later time, when the user clicks on the button.

In case JSX is getting in the way, here's the same code written in compiled JavaScript:

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

## Specifying Arguments

Here's a conundrum? for you: what if we want to specify *arguments* to this function?

For example, let's suppose that our function is called `setTheme`, and we'll use it to change the user's color theme from Light Mode to Dark Mode.

In order for this to work, we need to supply the name of the theme we're switching to, like this:

```js
// Switch to light mode:
setTheme('light');

// Switch to dark mode:
setTheme('dark');
```

How do we do this, though?

If we pass `setTheme` as a *reference* to `onClick`, we can't supply arguments:

```jsx
// 🚫 Invalid: calls the function without 'dark'
<button onClick={setTheme}>
  Toggle theme
</button>
```

In order to specify the argument, we need to wrap it in parentheses, but then it gets called right away:

```jsx
// 🚫 Invalid: calls the function right away
<button onClick={setTheme('dark')}>
  Toggle theme
</button>
```

We can solve this problem with a **wrapper function:**

```jsx
// ✅ Valid:
<button onClick={() => setTheme('dark')}>
  Toggle theme
</button>
```

We're creating a brand-new anonymous arrow function, `() => setTheme('dark')`, and passing it to React. When the user clicks the button, the function will run, and the code *inside* the function is executed (`setTheme('dark')`).

**Is creating anonymous functions like this bad for performance ? **

You may have heard that creating anonymous functions like this is bad for performance.

To summarise:

- There isn't a measurable performance difference between creating  anonymous functions vs. named functions. Same thing for arrow vs.  traditional functions.
- The cost of creating a new function is extremely low. Even low-end devices can create *hundreds of thousands* of functions in the span of a human blink.
- React has a built-in event delegation system that implements a bunch of optimizations for us.



