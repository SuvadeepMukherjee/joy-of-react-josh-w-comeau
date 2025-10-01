# Chap 3 | Hello React

------

#### Hello World React Application

------

The following pieces of code creates a **hello world** react application

 index.js ➡️

```js
// 1. Import dependencies
import React from 'react';
import { createRoot } from 'react-dom/client';

// 2. Create a React element
const element = React.createElement(
  'p',
  { id: 'hello' },
  'Hello World!'
);

// 3. Render the application
const container = document.querySelector('#root');
const root = createRoot(container);
root.render(element);
```

Index.html ➡️

```html
 <html>
<body>

  <div id="root"></div>

</body>
</html>
```

**Imporing dependencies**

```js
import React from 'react';
import { createRoot } from 'react-dom/client';
```

- We are importing the core *React* library from the react dependency as well as a *createRoot* function from react-dom
- We have the core react package and then there are different platform-specific renderers 
  - *react-dom* for the web
  - *react-native* for the mobile (iOS/Android) or desktop (Windows/macOS) applications
  - *react-three-fiber* for 3D scenes using WebGL and Three.js
- Every platform has its own "primitives" the set of built-in elements we use to create our User Interface
- All of these platforms will use the same core React framework which comes from the react package .But when it comes to actually turning all of the business logic into a user interface we need the correct bindings for our platform 

Note ➡️

What is the DOM ? 

- The DOM is the living , breathing structure that makes up a live website the browser turns the static HTML into the DOM
- Here is another way to look at it 
  - When we right-click and "View Source" we view the HTML , a static document that describes what will be constructed
  - When we right-click and inspect element we open the Elements pane and we are interacting with the DOM.We can change attributes and watch the UI update in response
- When we use a tool like React it works by interacting with the DOM via JavaScript.It will create , update and delete DOM elements as required

**Create a React element**

```js
const element = React.createElement(
  'p',
  { id: 'hello' },
  'Hello World!'
);
```

`React.createElement` is a function that accepts 3 or more arguments:

1. The type of the element to create.
2. The properties we want this element to have.
3. The element's contents. We can omit this if the element should be empty.

This function returns a “React element”. React elements are plain old JavaScript objects. If we inspect it with `console.log(element)`, we'll see something like this:

```js
{
  type: "p",
  key: null,
  ref: null,
  props: {
    id: 'hello',
    children: 'Hello World!',
  },
  _owner: null,
  _store: { validated: false }
}
```

This JavaScript object is a *description* of a hypothetical paragraph tag, with an ID of `hello`, containing the text `"Hello World!"`. This information will be used to construct the *actual* paragraph we can see in-browser.

Note ➡️ The final two properties _owner and _store are meant to be used internally by React , and can be safely ignored by us



Note ➡️

The DOM is organized as a family tree. Much like a family tree individual elements can have parents and grandparents , siblings and children . React elements can also specify children .React elements form a hierarchy just like DOM elements do

**Render the application**

```js
const container = document.querySelector('#root');
const root = createRoot(container);
root.render(element);
```

*document.querySelector* lets us capture a reference to a pre-existing DOM element . It works in this case because our index.html file includes the following element

```html
<div id="root"></div>
```

With react-dom's *createRoot* function we specify that this element should be the root of our application. And finally we render the application with *root.render(element)*. render function converts React elements into DOM nodes
