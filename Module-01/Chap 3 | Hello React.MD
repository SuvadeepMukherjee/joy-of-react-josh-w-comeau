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
