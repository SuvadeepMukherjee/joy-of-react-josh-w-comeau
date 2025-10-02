```

```

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

------

**Q7**: Why will we get a Javascript Syntax error if we try and place a statement in an expression slot ? 

**Solution**:

- Because JSX turns into *React.createElement()* function calls , we will get a JS syntax error if we try and place a statement in that slot .It has to be an expression

------

**Q8**:What is a JSX expression slot ? 

**Solution**:

- In JSX, the content we put between open/close tags is treated as a  static string. If we try and reference a variable, it'll print the  variable name itself, rather than the value it references.
- We can create *expression slots* with curly brackets (`{}`). Anything placed in-between curly brackets will be treated as pure JavaScript, instead of a string.
- Because JSX turns into `React.createElement()` function calls, we'll get a JavaScript syntax error if we try and place a statement in that slot. It has to be an *expression*.

------

**Q9**:How to add comments in JSX ? 

**Solution**:

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

------

```js
const userEmail = 'sumeet@thegreat.com';


const element = (

  <main id={userEmail.replace('@', '-')}>
    Hello World
  </main>

);
```

**Q10**:Does the code actually get evaluated during transpiling ? 

**Solution**:

when we transpile the code, it doesn't actually get *evaluated.* We've written some logic which will turn that `userEmail` string into `"sumeet-thegreat.com"`, replacing the `@` character with a `-`, but that only happens when we *run* the code.

When JSX gets transpiled to JS, we copy over everything between the `{` and `}`. We don't call any functions or run any of the logic. That happens later, when the processed JavaScript runs in the browser.

This is the distinction between *transpile-time* (the code processing that happens before the code runs in the browser) and *run-time* (the code execution that happens in the browser).

```js
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

------

**Q11**:We are supplying attributes in expression slots .What will the JSX expression be converted into (at runtime)? 

```jsx
<input required ={true}/>
<input type ="range" min={1} max={20}/>
```

**Solution**:

- React will automatically convert types as needed when supplying attributes in expression slots
- In HTML , values must be strings and so the boolean true is converted to the string "true"
- Similarly the 1 and 20 is converted to "1" and "20"

```jsx
// This works:
<input required="true" />


// And so does this!
<input required={true} />

// ✅ Valid
<input type="range" min="1" max="20" />
// ✅ Valid
<input type="range" min={1} max={20} />
```

------

**Q12**:

In HTML its possible to set attributes to true by specifying only the key 

```html
<input required > 
```

This same pattern has been implemented in JSX ; these two elements are equivalent 

```jsx
<input required />
<input required ={true}/>
```

However its not recommended doing this and preferred to spell it out and write `required={true}`

Why ? 

**Solution**:

```js
const name = 'Spot';
const dog = { name };


console.log(dog);
→ { name: 'Spot' }
```

When it comes to JavaScript objects, `{ name }` is equivalent to `{ name: name }`, and not `{ name: true }`.

JSX is a bit of a hybrid between HTML and JS, and so it can be ambiguous  for new developers. If we are  looking at JSX through an HTML lens, we would  expect the value to be `true`, but if you look at it through a JavaScript lens, you'd expect the value to be equal to the key.

In fact, there was even some talk about deprecating the “attribute-only”  syntax in JSX, to remove this ambiguity. In the end, the team decided to keep it for now, but it wouldn't be a surprise  if this was removed in the future.

And so, to keep things as simple and future-proof as possible, it is recommended to write the full thing out, `required={true}`.

------

**Q13**:

Difference between HTML and JSX 

- We cant use JS reserved words in JSX(HTML attributes sometimes overlap with JS reserved words)

  - for (html) changes to ______(JSX)

  - class(html) changes to ______(JSX)

- In HTML even if we dont close a tag .The browser is smart enough to figure out that the tag must be closed. In JSX ____

-  attributes rule

  - In JSX attributes need to be ___
    - autoplay(HTML)  ➡️ ___(JSX)
    - onclick(HTML) ➡️ ___(JSX)
    - tabindex(HTML) ➡️ ___(JSX)
    - stroke-dash-array ➡️ ____ (JSX)
  - Note ➡️ There are two exceptions to this rule : ____ and ____

- inline styles 

  - in HTML the style attribute allows us to apply some styles inline to a specified element . In JSX style  takes an ____
  - All CSS properties are written in ____
  - React will automatically apply the __  for certain CSS properties 

**Solution**:

- We cant use JS reserved words in JSX(HTML attributes sometimes overlap with JS reserved words)

  - for (html) changes to htmlFor(JSX)
  - class(html) changes to className(JSX)

  - Note ➡️ To be a bit more specific : for and class work fine when we use them on native HTML elements but we run into problems if we try and use them on custom components

- In HTML even if we dont close a tag .The browser is smart enough to figure out that the tag must be closed. In JSX we need to close every tag we open . In HTML the img tag does'nt need to be closed .In JSX we have to explicitly close this tag
- Case-sensitive attributes
  - In JSX attributes need to be camelCase
    - autoplay(HTML)  ➡️ autoPlay(JSX)
    - onclick(HTML) ➡️ onClick(JSX)
    - tabindex(HTML) ➡️ tabIndex(JSX)
    - stroke-dash-array ➡️ strokeDashArray (JSX)
  - Note ➡️ There are two exceptions to this rule : data attributes and ARIA attributes
- inline styles 
  - in HTML the style attribute allows us to apply some styles inline to a specified element . In JSX style instead takes an object
  - All CSS properties are written in camelCase. Every dash is replaced by capitalizing the subsequent word 
    - background-position becomes backgroundPosition
  - React will automatically apply the px suffix for certain CSS properties (Its a common convention in React to use unitless values where possible but we can absolutely use full units if we prefer)

------

**Q14**:

How to add whitespace in JSX ? 

**Solution**:

- To add whitespace in JSX we use the following pieces of code `{" "}`

Note ➡️ 

- Prettier is a tool that will automatically format our code for us.  It's opinionated, and follows conventions used by the majority of  JavaScript developers.
- Prettier knows about this whitespace gotcha, and will automatically add the `{' '}` whitespace character for us when it's necessary.

