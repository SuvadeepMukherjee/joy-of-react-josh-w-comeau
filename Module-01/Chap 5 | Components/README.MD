# Chap 5 | Components

#### **Components**:

- React is a component based framework 
- A component is a bundle of markup, style and logic that controls everything about a specific part of the user interface
- In React components are the main mechanism of reuse

------

#### Basic Syntax

Example: 

```jsx
import React from 'react';
import { createRoot } from 'react-dom/client';

function FriendlyGreeting() {
  return (
    <p
      style={{
        fontSize: '1.25rem',
        textAlign: 'center',
        color: 'sienna',
      }}
    >
      Greetings, weary traveller!
    </p>
  );
}

const container = document.querySelector('#root');
const root = createRoot(container);
root.render(<FriendlyGreeting />);
```

Components are defined as JS functions .Typically React components return one or more React elements.We render a component the same way we render an HTML tag.Instead of rendering a `<div>` or an `<h1>` we render `<FriendlyGreeting/>` . Both arrow functions and traditional functions work exactly the same way\

**The Big Component Rule**:

React components need to start with a capital letter .This is how the JSX compiler can tell whether we are trying to render a built-in HTML tag or a custom React component  . We have to enforce this rule because even if we have a new rule say Components can't share names with HTML tags but this will not work because every now and then we get a new HTML tag! . The list of HTML tags isn't set in stone. 

The convention is to use PascalCase for all React component names

Note ➡️ (VVIP)

```jsx
const heading = <h1>Hello!</h1>
const greeting = <FriendlyGreeting />
```

those same elements, compiled into JavaScript:

```js
const heading = React.createElement('h1', null, 'Hello!');
const greeting = React.createElement(FriendlyGreeting, null);
```

A React element is a description of a thing we want to create. In some cases, we want to create a DOM node, like an `<h1>` or a `<p>`. In other cases, we want to create a *component instance*.

The first argument that we pass to `React.createElement` is the “type” of the thing we want to create. For the first element, it's a string (`"h1"`). For the second element, it's a *function!* It's `FriendlyGreeting`, and not `"FriendlyGreeting"`.

---

## Props

Props are like arguments to a function:they allow us to pass data to our components, so that the component can include customizations based on the data

```jsx
import React from 'react';
import { createRoot } from 'react-dom/client';

function FriendlyGreeting({ name }) {
  return (
    <p
      style={{
        fontSize: '1.25rem',
        textAlign: 'center',
        color: 'sienna',
      }}
    >
      Greetings, {name}!
    </p>
  );
}

// The video was filmed using React 17, but this code
// has been updated for React 19:
const root = createRoot(document.querySelector('#root'));

root.render(
  <div>
    <FriendlyGreeting name="Josh" />
    <FriendlyGreeting name="Anita" />
    <FriendlyGreeting name="Rahul" />
  </div>
)
```

## Default values

To pass default values to props we do the following 

```jsx
function FriendlyGreeting({ name = 'there' }) {

  return (

<p>

  Hey {name}

</p>

  );

}
```
