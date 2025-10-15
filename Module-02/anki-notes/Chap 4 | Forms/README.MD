# Chap 4 | Forms

Anki => 04-forms

**Q1:** What is data binding and how does React handle data binding ?

**Solution:**

When building web applications, we often want to sync a bit of state to a particular form input. For example, a "username" field should be bound to the value of a username` state variable.

This is commonly known as “data binding”

Here's what this typically looks like in React:

```jsx
import React from 'react';

function SearchForm() {
  const [searchTerm, setSearchTerm] = React.useState('cats');

  return (
    <>
      <form>
        <label htmlFor="search-input">
          Search:
        </label>
        <input
          type="text"
          id="search-input"
          value={searchTerm}
          onChange={(event) => {
            setSearchTerm(event.target.value);
          }}
        />
      </form>
      <p>
        Search term: {searchTerm}
      </p>
       </>
  );
}

export default SearchForm;
```

**Q2**:What is the difference between synthetic events and events ? 

**Solution:**

React uses a "synthetic" event system .The events are special objects created by React, *not* the standard events used in JavaScript.

**Q3**:Why does React use "synthetic events ?"

**Solution**:

- It can ensure consistency, removing some edge-case issues with native 
  events being implemented slightly differently between browsers.

- It can include a few helpful “extras”, to improve the developer experience

- In earlier versions, this event system had a slight beneficial impact on 
  performance, though those changes have since been removed. If you see 
  references online to “event pooling” or `event.persist()`, you can ignore them, since this system was removed in React 17.

**Q4**:How to access real events in React ? 

**Solution:**

```jsxag-0-1j7jjjupkag-1-1j7jjjupk
<input
  onChange={(event) => {
    const realEvent = event.nativeEvent;

    console.log(realEvent); // DOM InputEvent object
  }}
/>ag-0-1j7jjjupkag-1-1j7jjjupk
```

**Q5**:Controlled vs Uncontrolled input

**Solution:**

When we set the `value` attribute on a form input, we tell React that it should be a *controlled input*. The word “controlled” has a specific definition in React; it means that React is managing the input.

By contrast, if we don't set `value`, the input is an *uncontrolled input*. This means that React doesn't do any management.

**Q6**:An input should always either be controlled or uncontrolled

is this statement true ? 

**Solution:**

There's a golden rule here: **An input should always either be controlled or uncontrolled.** React doesn't like when we flip an element from one to the other.

**Q7**:Consider this situation:

```jsx
import React from 'react';

function SignupForm() {
  // No default value:
  const [username, setUsername] = React.useState();

  return (
    <form>
      <label htmlFor="username">
        Select a username:
      </label>
      <input
        type="text"
        id="username"
        value={username}
        onChange={event => {
          setUsername(event.target.value);
        }}
      />
    </form>
  );
}

export default SignupForm;
```

If we type in the input box we will get the following warning

![](../../assets/controlled-uncontrolled-warning.png)

This input *is* controlled! We're setting `value={username}` from the very first render!

But why are we getting this error and how to fix it

**Solution**:

**Here's the problem:** `username` is undefined at first, since there is no default value in the state hook. Here's a simplified version of what we're doing:

```jsx
username = undefined;

<input
  type="text"
  id="username"
  value={username}
  onChange={event => {
    setUsername(event.target.value);
  }}
/>
```

When we set `value` to `undefined`, it's the same as not setting it at all. React will treat the input as an *uncontrolled input*.

When the user starts typing in the input, the `onChange` event updates the value of `username` from `undefined` to a string. And so, React flips the element to a *controlled input*, and raises the warning.

**Here's how to solve the problem:** We always want to make sure we're passing a defined `value`. We can do this by initializing `username` to an empty string:

```jsx
// 🚫 Incorrect. `username` will flip from `undefined` to a string:
const [username, setUsername] = React.useState();

// ✅ Correct. `username` will always be a string:
const [username, setUsername] = React.useState('');
```

With this change, our input is being controlled by React state from the 
very first render, since we're always passing a defined value. Even 
though empty strings are considered falsy, they still “count” when it comes to controlling React inputs.
