## Module 1 (React Fundamentals)

Anki Tag ➡️ 01-react-fundamentals

#### Q1:Create a "hello world" react application with React ? 

**Solution**:

The following code snippets creates a **hello world** react application

Index.js ➡️

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

------

```js
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

#### Q2:This is the react code to print "hello world", why do we need to import 2 dependencies ? 

**Solution**:

- We are importing the core *React* library from the react dependency as well as a *createRoot* function from react-dom
- We have the core react package and then there are different platform-specific renderers 
  - *react-dom* for the web
  - *react-native* for the mobile (iOS/Android) or desktop (Windows/macOS) applications
  - *react-three-fiber* for 3D scenes using WebGL and Three.js
- Every platform has its own "primitives" the set of built-in elements we use to create our User Interface
- All of these platforms will use the same core React framework which comes from the react package .But when it comes to actually turning all of the business logic into a user interface we need the correct bindings for our platform 

#### Q3:What is DOM (Document Object Model) ? 

**Solution**:

- The DOM is the living , breathing structure that makes up a live website the browser turns the static HTML into the DOM
- Here is another way to look at it 
  - When we right-click and "View Source" we view the HTML , a static document that describes what will be constructed
  - When we right-click and inspect element we open the Elements pane and we are interacting with the DOM.We can change attributes and watch the UI update in response

#### Q4:Fill in the blanks 

When we use a tool like React it works by interacting with the _____ via JavaScript.It will create , update and delete ____ elements as required

**Solution**:

When we use a tool like React it works by interacting with the DOM via JavaScript.It will create , update and delete DOM elements as required
