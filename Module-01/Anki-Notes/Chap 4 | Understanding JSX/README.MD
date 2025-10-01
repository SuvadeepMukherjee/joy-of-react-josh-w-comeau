# Chap 4 | Understanding JSX

Anki Tag ➡️ 04-understanding-jsx

**Q1**:Create a hypothetical paragraph with an id ="hello" that contains the text "hello world" using jsx

**Solution**:

index.html ➡️

```html
<html>
<body>

  <div id="root"></div>

</body>
</html> 
```

index.js ➡️

```jsx
import React from 'react';
import { createRoot } from 'react-dom/client';

const element = (

  <p id="hello">
    Hello World!
  </p>


);

const container = document.querySelector('#root');
const root = createRoot(container);
root.render(element);
```

Why the parenthesis ? 

In the example above, we wrap the JSX in parentheses, like this:

```jsx
const element = (
  <nav id="main-nav">
    <ul>
      (List items removed, for brevity)
    </ul>
  </nav>
);
```

This is done purely for formatting purposes. It allows us to push the JSX onto the next line.

------

**Q2**: Why do we use JSX instead of *React.createElement* ? 

**Solution**:

React elements can form a tree structure (This happens when we set the children parameter of a React element to another React element )

In practise we often wind up with pretty significant tree structure in our React code

```js
const element = React.createElement(
  "nav",
  { id: "main-nav" },
  React.createElement(
    "ul",
    null,
    React.createElement(
      "li",
      null,
      React.createElement(
        "a",
        { href: "/" },
        "Home"
      )
    ),
    React.createElement(
      "li",
      null,
      React.createElement(
        "a",
        { href: "/archives" },
        "Archives"
      )
    )
  )
);
```

------

**Q3**: What do you mean by the statement JSX needs to be transpiled to JS ?

**Solution**:

JS Engines dont understand JSX they only understand JS so we need to transpile our code into plain js .This is most commonly done as part of a build step using a tool called Babel .The JSX we write gets converted into *React.createElement* .By the time our code is running in the user's browser all of the JSX has been zapped out and we are left with a JS file full of nested *React.createElement* calls

------

**Q4**:Can we include jsx in a js file ? 

**Solution**:

In the early days of React , any file that included JSX had to use the .jsx file extension .This was how we told the compiler that this file had some JSX in it and needs to be compiled to JS 

To ease out the friction in the development process the rules were loosened .Now we can include JSX in a .js file and everything will work perfectly (One exception is if you use Vite , a bundler created by the Vue team . This tool still enforces the no-JSX-in-JS-Files rule)

------

**Q5**: Can we skip the React import ? 

```jsx
import React from 'react';
import { createRoot } from 'react-dom/client';


const element = (

  <p id="hello">
    Hello World!
  </p>

);


const container = document.querySelector('#root');
const root = createRoot(container);
root.render(element);
```

On the very first line, we're importing `React`, but we aren't actually using it anywhere…  Can we omit it?

**Solution**:

After we compile away the JSX, we're left with the following code:

```jsx
import React from 'react';
import { createRoot } from 'react-dom/client';


const element = React.createElement(
  'p',
  { id: 'hello' },
  'Hello World!'
);


const container = document.querySelector('#root');
const root = createRoot(container);
root.render(element);
```

When the JSX is compiled into plain JS, the dependency makes itself clear. That `<p>` tag becomes a `React.createElement` call! It's obfuscated by the JSX.

In earlier versions of React, you'd get an error if you forgot to include the React import:

![01-react-is-not-defined](../../assets/01-react-is-not-defined.png)

This error message produced a lot of confusion for beginners.This was such a common stumbling block for beginners that the React Team in React 17 introduced a new “JSX transformer”, used by Babel and other compilers. Essentially, it *automatically injects the import* during the build process.

For example, let's suppose we had this code:

```jsx
const element = (

  <p id="hello">
    Hello World!
  </p>

);
```

Using the modern JSX transformer, it will get compiled to:

```jsx
import { jsx as _jsx } from 'react/jsx-runtime';


const element = _jsx(
  'p',
  { id: 'hello' },
  'Hello World!'
);
```

`_jsx` is a fancy optimized version of `React.createElement`. It includes some shortcuts when we use certain React features like  Fragments or Portals. Otherwise, it does the exact same thing as `React.createElement`: it creates a React element.

And so, these days, we *don't* have to import React. The JSX compiler will solve this problem for us.

------

**Q6**: What is the JSX transformer ? 

**Solution**:

JSX transformer automatically injects the React import during the build process.The JSX transformer is used by Babel and other compilers 

For example, let's suppose we had this code: 

```html
const element = (

  <p id="hello">
    Hello World!
  </p>


);
```

Using the modern JSX transformer, it will get compiled to:

```js
import { jsx as _jsx } from 'react/jsx-runtime';


const element = _jsx(
  'p',
  { id: 'hello' },
  'Hello World!'
);
```


`_jsx` is a fancy optimized version of `React.createElement`. It includes some shortcuts when we use certain React features like  Fragments or Portals. Otherwise, it does the exact same thing as `React.createElement`: it creates a React element.