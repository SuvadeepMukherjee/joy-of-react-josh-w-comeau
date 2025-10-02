# Understanding JSX

Very few developers create elements using *React.createElement* .It's much more common to use a specialized syntax called JSX

Hello World using JSX ➡️

index.html ➡️

```html
<html>
<body>

  <div id="root"></div>

</body>
</html>
```

index.js ➡️

```js
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



Note ➡️ VVIP 

**Why do we need JSX ?** 

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

**Transpiling JSX into JS**
JS Engines dont understand JSX they only understand JS so we need to transpile our code into plain js .This is most commonly done as part of a build step using a tool called Babel .The JSX we write gets converted into *React.createElement* .By the time our code is running in the user's browser all of the JSX has been zapped out and we are left with a JS file full of nested *React.createElement* calls

Note ➡️ (VVIP)

**Transpiling vs Compiling**

compiling ➡️ Process of taking human-readable code and transforming it into machine-readable code . A compiler might take a high-level language like Python and convert it into machine code , low-level instructions that appear like gibberish to us 

transpiling ➡️ Taking one high level language and transforming it into another high-level language . For example we might transpile JS into python

**File extensions and JSX**

In the early days of React , any file that included JSX had to use the .jsx file extension .This was how we told the compiler that this file had some JSX in it and needs to be compiled to JS 

To ease out the friction in the development process the rules were loosened .Now we can include JSX in a .js file and everything will work perfectly (One exception is if you use Vite , a bundler created by the Vue team . This tool still enforces the no-JSX-in-JS-Files rule)

**Can we skip the React import ? **

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

After we compile away the JSX, we're left with the following code:

```js
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

![01-react-is-not-defined](../assets/01-react-is-not-defined.png)

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

**Expression Slots**:

- In JSX, the content we put between open/close tags is treated as a static  string. If we try and reference a variable, it'll print the variable  name itself, rather than the value it references.
- We can create *expression slots* with curly brackets (`{}`). Anything placed in-between curly brackets will be treated as pure JavaScript, instead of a string.
- There aren't a lot of rules when it comes to JSX. The compilation process  doesn't check if it's even valid! It's the messenger; it forwards the  content along to the pure JS output.
- Because JSX turns into `React.createElement()` function calls, we'll get a JavaScript syntax error if we try and place a statement in that slot. It has to be an *expression*.

**Comments in JSX**:

To add a comment in JSX, we use an expression slot:

```jsx
const element = (

  <div>
    {/* Some comment! */}
  </div>

);
```

We specifically need to use the multi-line comment syntax (`/* */`) instead of the single-line syntax (`//`). This is because the single-line syntax comments *everything* out, including the closing `}` for the expression slot!

![Code snippet with a double-slash comment, not working](https://courses.joshwcomeau.com/react-mats/jsx-broken-comment.png)

**Attribute Expression Slots**:

To give dynamic attribute values we use expression slots

```jsx
const uniqueId = 'content-wrapper';


const element = (
  <main id={uniqueId}>
    Hello World
  </main>
);
```

 The squiggly brackets (`{}`) allow us to create an *expression slot*. This time, we're creating a slot for the value of the `id` attribute.

Here's how it compiles : 

```js
const element = React.createElement(
  'main',
  {
    id: uniqueId,
  },
  'Hello World'
);
```

We can use attribute expression slots whenever we need the values to be dynamic. We can put *any valid JavaScript* expression in here, not just variables:

```jsx
const userEmail = 'sumeet@thegreat.com';


const element = (
  <main id={userEmail.replace('@', '-')}>
    Hello World
  </main>
);


// Will get compiled as:
const compiledElement = React.createElement(
  'main',
  {
    id: userEmail.replace('@', '-'),
  },
  'Hello World'
);
```

Note that when we transpile the code, it doesn't actually get *evaluated.* We've written some logic which will turn that `userEmail` string into `"sumeet-thegreat.com"`, replacing the `@` character with a `-`, but that only happens when we *run* the code.

When JSX gets transpiled to JS, we copy over everything between the `{` and `}`. We don't call any functions or run any of the logic. That happens later, when the processed JavaScript runs in the browser.

This is the distinction between *transpile-time* (the code processing that happens before the code runs in the browser) and *run-time* (the code execution that happens in the browser).

**Type Coercion**:(We are supplying attributes in expression slots.What will the JSX expression be converted  into ar runtime )? 

- React will automatically convert types as needed when supplying attributes in expression slots
- In HTML , values must be strings and so the boolean true is converted to the string "true"
- Similarly the 1 and 20 is converted to "1" and "20"

```html
// This works:
<input required="true" />


// And so does this!
<input required={true} />

// ✅ Valid
<input type="range" min="1" max="20" />
// ✅ Valid
<input type="range" min={1} max={20} />
```

