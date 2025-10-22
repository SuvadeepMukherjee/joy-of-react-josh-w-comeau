## Chap 3 | Hello World

Anki Tag ➡️ 03-hello-world

**Q1**:Create a React application that paints to the browser "hello world" with `React.createElement` ? 

**Solution**:

The following code snippets creates a **hello world** react application

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

index.html ➡️

```html
<html>
<body>

  <div id="root"></div>

</body>
</html>
```

------

**Q2**:

index.js ➡️

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

index.html ➡️

```html
<html>
<body>

  <div id="root"></div>

</body>
</html>

```

The above code snippet will produce the text "hello world", why do we need to import 2 dependencies (`React` and `react-dom`)? 

**Solution**:

- We are importing the core *React* library from the react dependency as well as a *createRoot* function from react-dom
- We have the core react package and then there are different platform-specific renderers 
  - *react-dom* for the web
  - *react-native* for the mobile (iOS/Android) or desktop (Windows/macOS) applications
  - *react-three-fiber* for 3D scenes using WebGL and Three.js
- Every platform has its own "primitives" the set of built-in elements we use to create our User Interface
- All of these platforms will use the same core React framework which comes from the react package .But when it comes to actually turning all of the business logic into a user interface we need the correct bindings for our platform 

------

Q3:What is DOM (Document Object Model) ? 

**Solution**:

- The DOM is the living , breathing structure that makes up a live website / web application the browser turns the static HTML into the DOM
- Here is another way to look at it 
  - When we right-click and "View Source" we view the HTML , a static document that describes what will be constructed
  - When we right-click and inspect element we open the Elements pane and we are interacting with the DOM.We can change attributes and watch the UI update in response

------

Q4:Fill in the blanks 

When we use a tool like React it works by interacting with the _______ via JavaScript.It will create , update and delete ____ elements as required

**Solution**:

When we use a tool like React it works by interacting with the DOM via JavaScript.It will create , update and delete DOM elements as required

------

Q5:Create a React element with `React.createElement` that is a description of a hypothetical paragraph tag with an id of hello containing the text 'Hello World!'

**Solution**:

The following JS code is the description of a hypothetical paragraph tag with an ID of hello containing the text "hello World!"

```js
const element = React.createElement(
  'p',
  { id: 'hello' },
  'Hello World!'
);
```

*React.createElement* is a function that accepts 3 or more arguments 

- The type of the element to create
- The propertied we want this element to have
- The element's content .We can omit this if the element should be empty

------

Q6:The following code snippet creates a React Element with React.createElement .What are React Elements ? 

```js
const element = React.createElement(
  'p',
  { id: 'hello' },
  'Hello World!'
);
```

**Solution**:

React elements are plain old JS objects 

------

Q7:What will the console log show ? 

```js
const element = React.createElement(
  'p',
  { id: 'hello' },
  'Hello World!'
);
console.log(element)
```

**Solution**:

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

 Note ➡️ The final two properties _owner and _store are meant to be used internally by React and can be safely ignored by us 

------

Q8:Fill in the blanks 

The DOM is organized as. ___

**Solution**:

The DOM is organized as a family tree .

Much like a family tree , individual elements can have parents and grandparents , siblings and children 

React elements can also specify children . React elements form a hierarchy just like DOM elements do 

------

**Q9**:

index.js ➡️

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

Index.html➡️

```html
<html>
<body>

  <div id="root"></div>

</body>
</html>
```



This is the React code to print "hello world". Explain the following lines of code

```js
const container = document.querySelector('#root');
const root = createRoot(container);
root.render(element);
```

**Solution**:

*document.querySelector* lets us capture a reference to a pre-existing DOM element . It works in this case because our index.html file includes the following element

```html
<div id="root"></div>
```

With react-dom's *createRoot* function we specify that this element should be the root of our application. And finally we render the application with *root.render(element)*. render function converts React elements into DOM nodes

------

**Q10**:

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

index.html ➡️

```html
<html>
<body>

  <div id="root"></div>

</body>
</html>
```

The following code snippets print the world "hello world" to the browser , How exactly is our code running, when we don't have a `<script src="/index.js"></script>`?

**Solution**:

The `index.js` file, along with the dependencies (React, React DOM), are bundled into cryptically-named files like `2.61b71a6f1.chunk.js`, and a `<script>` tag is dynamically injected into the HTML.There's an *active process* that transforms the files we write.

------

**Q11**:

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

index.html ➡️

```html
<html>
<body>

  <div id="root"></div>

</body>
</html>
```

The following code snippets print the world "hello world" to the browser , Our code is running, even though we don't have a `<script src="/index.js"></script>` because of the following reason : 

**The `index.js` file, along with the dependencies (React, React DOM), are bundled into cryptically-named files like `2.61b71a6f1.chunk.js`, and a `<script>` tag is dynamically injected into the HTML.There's an *active process* that transforms the files we write.**

Why can't we add our code ourselves, using `<script>` tags, the way we do when working with jQuery, Vanilla JS and other libraries?

**Solution**:

There are a few reasons for this:

 ➡️  JSX needs to be compiled into JS

➡️ It enables quality-of-life improvements, like being able to use bleeding-edge JS features

➡️ It's necessary for good performance, since React apps often have hundreds or even thousands of JS files that need to be bundled together.