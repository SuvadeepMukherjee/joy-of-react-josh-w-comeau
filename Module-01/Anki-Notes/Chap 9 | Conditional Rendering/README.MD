# Chap 9 | Conditional Rendering

Anki ➡️09-conditional-rendering

**Q1**:We want to conditionally render with an if statement .

- But why this sort of thing is illelegal
- Fix the code such that we can conditionally render with an if statement 

*App.js* ➡️⬇️

```jsx
function Friend({ name, isOnline }) {
  return (
    <li className="friend">
      {if (isOnline) {
        <div className="green-dot" />
      }}

      {name}
    </li>
  );
}
```

*Friend.js* ➡️⬇️

```js
function Friend({ name, isOnline }) {
  return (
    <li className="friend">
      {if (isOnline) {
        <div className="green-dot" />
      }}

      {name}
    </li>
  );
}
```



**Solution**:

Lets consider how this would compile down to JavaScript

```js
function Friend({ name, isOnline }) {
  return React.createElement(
    'li',
    { className: 'friend' },
    if (isOnline) {
      React.createElement('div', { className: 'green-dot' });
    },
    name
  );
}
```

But we cant put an if statement in the middle of a function call like this . This would be equivalent to this 

```js
console.log(
  if (isLoggedIn) {
    "Logged in!"
  } else {
    "Not logged in"
  }
)
```

We can still use an if statement.But we have to pull it up so that it's not in the middle of a `React.createElement` call : 

Friend.js ➡️⬇️

```js
function Friend({ name, isOnline }) {
  let prefix;

  if (isOnline) {
    prefix = <div className="green-dot" />;
  }

  return (
    <li className="friend">
      {prefix}
      {name}
    </li>
  );
}

export default Friend;
```

There is no rule that says that our JSX has to be part of the return statement.We can absolutely assign chunks of JSX to a variable , anywhere within our component defination!

The JSX compiles to this:

```jsx
function Friend({ name, isOnline }) {
  let prefix;

  if (isOnline) {
    prefix = React.createElement(
      'div',
      { className: 'green-dot' }
    );
  }

  return React.createElement(
    'li',
    { className: 'friend' },
    prefix,
    name
  );
}
```

In the code above , `prefix` will either be assigned to a React element, or it won't be defined at all .This works because React ignores undefined values.

Note 💥

Consider the following code: 

```jsx
function Greeting() {
  let someClass;

  return (
    <div className={someClass}>
      Hello World
    </div>
  );
}
```

The following HTML will be produced:

```html
<div>
  Hello World
</div>
```

In the code above, a variable named `someClass` is created, but it isn't given a value. This means that it resolves to `undefined`.

When React sees that an attribute is being set to `undefined`, it omits that attribute entirely from the DOM node. Rather than give it an empty value like `className=""`, it acts as though we haven't even tried to set a value.

This is true for some other falsy values as well, like `null` and `false`.

**Q2**:For what values of someclass will the following HTML will be produced

*Gretting.js* ➡️

```js
function Greeting() {
  let someClass;

  return (
    <div className={someClass}>
      Hello World
    </div>
  );
}
```

HTML ➡️⬇️

```html
<div>
  Hello World
</div>
```

**Solution**:

In the code above, a variable named `someClass` is created, but it isn't given a value. This means that it resolves to `undefined`.

When React sees that an attribute is being set to `undefined`, it omits that attribute entirely from the DOM node. Rather than give it an empty value like `className=""`, it acts as though we haven't even tried to set a value.

This is true for some other falsy values as well, like `null` and `false`.