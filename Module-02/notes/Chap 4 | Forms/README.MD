# Chap 4 | Forms

# Data Binding

When building web applications, we often want to sync a bit of state to a particular form input. For example, a "username" field should be bound to the value of a username` state variable.

This is commonly known as “data binding”. Most front-end frameworks offer a 
way to bind a particular bit of state to a particular form control.

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

## Synthetic Events

React uses a "synthetic" event system .The events are special objects created by React, *not* the standard events used in JavaScript.

Why does React do this? Several reasons:

- It can ensure consistency, removing some edge-case issues with native 
  events being implemented slightly differently between browsers.

- It can include a few helpful “extras”, to improve the developer experience

- In earlier versions, this event system had a slight beneficial impact on 
  performance, though those changes have since been removed. If you see 
  references online to “event pooling” or `event.persist()`, you can ignore them, since this system was removed in React 17.

If you ever need to access the “real” event object, you can access it like this:

```jsx
<input
  onChange={(event) => {
    const realEvent = event.nativeEvent;

    console.log(realEvent); // DOM InputEvent object
  }}
/>
```

## Controlled vs Uncontrolled inputs

When we set the `value` attribute on a form input, we tell React that it should be a *controlled input*. The word “controlled” has a specific definition in React; it means that React is managing the input.

By contrast, if we don't set `value`, the input is an *uncontrolled input*. This means that React doesn't do any management.

There's a golden rule here: **An input should always either be controlled or uncontrolled.** React doesn't like when we flip an element from one to the other.

This can lead to a common footgun. Let's learn about it, so that we can avoid it.

Consider this situation:

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

If we type in the input box we will get the following  warning 

 ![](../../assets/controlled-uncontrolled-warning.png)

This input *is* controlled! We're setting `value={username}` from the very first render!

But why are we getting this error and how to fix it 

**Solution:** 

**Here's the problem:** `username` is undefined at first, since there is no default value in the state hook. Here's a simplified version of what we're doing:

```jsx
const username = undefined;

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

## The onClick Parable

Let's suppose we're buildinga search form:

App.js => 

```jsx
import SearchForm from './SearchForm';

function App() {
  // This function is a placeholder.
  function runSearch(searchTerm) {
    window.alert(`Searched for: ${searchTerm}`);
  }
  
  return (
    <SearchForm runSearch={runSearch} />
  );
}

export default App;
```

SearchForm.js => 

```js
import React from 'react';

function SearchForm({ runSearch }) {
  const [searchTerm, setSearchTerm] = React.useState('');
  
  return (
    <div className="search-form">
      <input
        type="text"
        value={searchTerm}
        onChange={event => {
          setSearchTerm(event.target.value);
        }}
      />
      <button>
        Search!
      </button>
    </div>
  );
}

export default SearchForm;
```

In this example, `runSearch` is the function we want to 
call when the user clicks/taps the "Search!" button. In Module 3, we'll 
learn how to make network requests, but for now, it's a stub?.

**Here's the question:** how should I use this function?

A lot of developers instinctively solve for this by adding an `onClick` handler to the submit button:

```js
<button onClick={() => runSearch(searchTerm)}>
  Search!
</button>
```

**There are a number of problems with this approach.** For example, what if the user tries to search by pressing "Enter" after typing in the text input?

Well… I suppose we could tackle that with an `onKeyDown` event listener?

```js
<input
  type="text"
  value={searchTerm}
  onChange={event => {
    setSearchTerm(event.target.value);
  }}
  onKeyDown={event => {
    if (event.key === 'Enter') {
      runSearch(searchTerm);
    }
  }}
/>
```

We're going down a bad road here. We're re-implementing stuff that the browser already knows how to do!

**Use a form:**

To solve this problem, along with so many others, we should wrap our form controls in a `<form>` tag.

Then, instead of listening for clicks and keys, we can listen for the *form submit event*.

Look how much simpler the code gets:

App.js => 

```js
import SearchForm from './SearchForm';

function App() {
  // This function is a placeholder.
  function runSearch(searchTerm) {
    window.alert(`Searched for: ${searchTerm}`);
  }
  
  return (
    <SearchForm runSearch={runSearch} />
  );
}

export default App;
```

SearchForm.js =>

```js
import React from 'react';

function SearchForm({ runSearch }) {
  const [searchTerm, setSearchTerm] = React.useState('');
  
  return (
    <form
      className="search-form"
      onSubmit={event => {
        event.preventDefault();
        runSearch(searchTerm);
      }}
    >
      <input
        type="text"
        value={searchTerm}
        onChange={event => {
          setSearchTerm(event.target.value);
        }}
      />
     <button>
        Search!
      </button>
    </form>
  );
}

export default SearchForm;
```

The form submit event will be called automatically when the user 
clicks the button, or presses "Enter" whenever the input or button is 
focused. When that event fires, we'll run our search.

Instead of trying to re-create a bunch of standard web platform stuff, we should *use the platform* and let it solve these sorts of problems for us!

By using a form submit event, we get to use *client-side validation*:

```html
<input
  type="password"
  required={true}
  minLength={8}
/>
```

**Default form behavior**:

Alright, so there *is* one little quirk with using `onSubmit`. We need to prevent the default submission behavior:

```html
<form
  className="search-form"
  onSubmit={event => {
    event.preventDefault();
    runSearch(searchTerm);
  }}
>
```

To understand why this is necessary, we need to take a trip back through time, to an era without client-side requests. Before `fetch`, before `XMLHttpRequest`, before JSON.

If
 you wanted to make a request to a server, like when fetching search 
results, you couldn't request only the data. You needed to request a 
whole new HTML file. Essentially, the user would be redirected to a new 
URL, and the server would then render a template into an HTML document, 
using the data sent with the request.

**Forms still operate this way by default.** When you submit a form, the browser will try to send the user to the URL specified by the `action` attribute:

```html
<!--
  Submitting this form will redirect the user to the
  /search page, sending along the data collected from
  the form fields.
-->
<form
  method="POST"
  action="/search"
>
```

If we omit the `action` attribute, the browser will use the *current* URL, effectively reloading the page.

In
 the context of a modern React application, this isn't usually what we 
want. We don't want to reload the entire page, we want to fetch a bit of
 data and re-render a few components with that data. This produces a 
faster, smoother user experience.

That's why we need to include `event.preventDefault()`. It stops the browser from executing a full page reload.



## Other Form Controls

In addition to the text inputs we've been using so far, the web platform provides *lots* of additional form controls. They include:

- Textareas

- Radio buttons

- Checkboxes

- Selects

- Ranges

- Color pickers

If you've ever had to work with these form controls outside of React, 
you've likely been surprised/frustrated by how dissimilar they are from one another.

For example, textareas define the current value *as children*, rather than using a `value` prop:

```html
<textarea>
  This is the current value
</textarea>
```

As another example, select tags use a `selected` prop on one of the `<option>` children to signify the selected value:

```html
<select>
  <option value="a">
    Option 1
  </option>
  <option value="b" selected>
    Option 2
  </option>
  <option value="c">
    Option 3
  </option>
</select>
```

**Here's the good news:** React has tweaked many of 
these form controls so that they all behave much more similarly. There's
 a lot less chaos with form controls in React.

Essentially, all form controls follow the same pattern:

1. The current value is locked using either `value` (for most inputs) or `checked` (for checkboxes and radio buttons).

2. We respond to changes with the `onChange` event listener.

## Select tag

The `<select>` tag allows the user to select a single option from a list of predefined options.When working with select tags in React, they work pretty much exactly like text inputs. We use `value` and `onChange`. Here's an example:

```jsx
import React from 'react';

function App() {
  const [
    selectedOption,
    setSelectedOption
  ] = React.useState('red');

  return (
    <form>
      <fieldset>
        <legend>
          What is your favourite color?
        </legend>
        
        <select
          value={selectedOption}
          onChange={event => {
            setSelectedOption(event.target.value)
          }}
        >
        <option value="red">
            Red
          </option>
          <option value="green">
            Green
          </option>
          <option value="blue">
            Blue
          </option>
        </select>
      </fieldset>
      
      <p>
        Selected value:
        <br />
        {selectedOption}
      </p>
    </form>
  );
}

export default App;
```

By setting the `value` prop, we make this a controlled component, binding the selected option to our React state. When we add the `onChange` event listener, we allow this state to be changed by selecting a different option from the list.

## Radio buttons

Alright, so radio buttons are a bit trickier.

Ostensibly, they serve the same purpose as a `<select>` tag; they allow the user to select 1 choice from a group of options.

The tricky thing, though, is that this state is split across multiple independent HTML elements. There's only one `<select>` tag, but there are multiple `<input type="radio">` buttons!

Let's start by looking at an example of a controlled radio button group in React:

```jsx
import React from 'react';

function App() {
  const [
    value,
    setValue
  ] = React.useState('no');

  return (
    <form>
      <fieldset>
        <legend>Do you agree?</legend>
        
        <input
          type="radio"
          name="agreed-to-terms"
          id="agreed-yes"
          value="yes"
          checked={value === "yes"}
          onChange={event => {
            setValue(event.target.value)
          }}
        />
        {' '}
        <label htmlFor="agreed-yes">
          Yes
        </label>
        <br />
        
        <input
          type="radio"
          name="agreed-to-terms"
          id="agreed-no"
          value="no"
          checked={value === "no"}
          onChange={event => {
            setValue(event.target.value)
          }}
        />
        {' '}
        <label htmlFor="agreed-no">
          No
        </label>

        </fieldset>
    </form>
  );
}

export default App;
```

All the properties : 

- `name` — The browser needs to know that each button is part of the same group,
   so that ticking one option will untick the others. This is done through
   the `name` prop. Each radio button in a group should share the same `name`.

- `value` — Each radio button has its own value. This property will be copied 
  over to our React state when the option is ticked. This is the 
  definition / meaning for each radio button.

- `id` — like other form controls, this is needed so that the `<label>` can be associated with the right input, so that clicking the label focuses the input.

- `checked` — This is the prop that binds a given radio button to our React state, 
  making it a controlled value. It should be set to a boolean value: `true` if it's ticked, `false` if it's not. Only one radio button should be set to `true` at a time.

Even though it looks quite different, it does follow the same rough formula. We use an `onChange` event listener to detect when a given option is ticked. When that happens, we update the React state.

For most other inputs, we bind React state to the `value` prop. In this case, though, we don't *have* a single `value` prop to bind to, since we have multiple radio buttons. So instead, we bind to `checked`, controlling the ticked/unticked status for each button in the group with React state.
