# Chap 9 | Conditional Rendering

Anki ➡️09-conditional-rendering

**Q1**:We want to conditionally render with an if statement .But why this sort of thing is illelegal ? 

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

