# Chap 5 | Props Vs State

## Props

“Props” is short for “properties”. At a micro level, they're like the attributes we place on HTML elements, like `class` or `href`:

```jsx
<a
  class="nav-link"
  href="/category/stuff"
>
```

For example, the `Button` component below takes a “variant” prop. This prop will be used internally to control styling, like how the `class` attribute works in HTML.

App.js => 

```jsx
import Button from './Button';

function App() {
  return (
    <div className="box">
      <p>
        Are you sure you want to continue?
      </p>
      <div className="actions">
        <Button variant="secondary">
          Cancel
        </Button>
        <Button variant="primary">
          Confirm
        </Button>
      </div>
    </div>
  );
}

export default App;
```

Button.js => 

```jsx
import React from 'react';

import styles from './Button.module.css';

function Button({ variant, children }) {
  return (
    <button
      className={`${styles.wrapper} ${styles[variant]}`}
    >
      {children}
    </button>
  )
}

export default Button;
```

Button.module.css => 

```css
.wrapper {
  padding: 8px 16px;
  font-size: 1rem;
  text-transform: uppercase;
  letter-spacing: 1px;
  border-radius: 8px;
  border: none;
  transition: background-color 200ms, color 200ms;
  cursor: pointer;
}

.primary {
  background-color: hsl(245deg 100% 60%);
  color: white;
}

.secondary {
  background-color: transparent;
  color: hsl(15deg 10% 30%);
}
.primary:hover {
  background-color: hsl(245deg 75% 50%);
}

.secondary:hover {
  background-color: hsl(15deg 60% 90% / 0.5);
  color: black;
}
```

Props allow us to customize the behaviour of a given component, so  that the exact same component can do different things in different  scenarios.

**Props are the inputs to our components**, like arguments passed to a function.

## State

In the example above, our application is completely static. Every  time we run this code, we get the same result. This will be true until  the heat death of the universe.

But what if we wanted stuff to change over time? That's where state comes in.

Let's tweak our example a bit:

App.js => 

```js
import React from 'react';

import Button from './Button';

function App() {
  const [hasAgreed, setHasAgreed] = React.useState(false);
  
  return (
    <div className="box">
      <p>
        Are you sure you want to continue?
      </p>
      <label htmlFor="confirm-checkbox">
        <span className="required">*</span>
        <input
          id="confirm-checkbox"
          type="checkbox"
          value={hasAgreed}
          onChange={() => setHasAgreed(!hasAgreed)}
        />
			<span>
          I agree with <a href="/terms">the terms</a>.
        </span>
      </label>
      <div className="actions">
        <Button
          variant="secondary"
          isEnabled={true}
        >
          Cancel
        </Button>
        <Button
          variant="primary"
          isEnabled={hasAgreed}
        >
          Confirm
        </Button>
      </div>
    </div>
  );
}
export default App;
```

Button.js ➡️

```js
import React from 'react';

import styles from './Button.module.css';

function Button({ variant, isEnabled, children }) {
  return (
    <button
      className={`${styles.wrapper} ${styles[variant]}`}
      disabled={!isEnabled}
    >
      {children}
    </button>
  )
}

export default Button;
```

Button.module.css ➡️

```css
.wrapper {
  padding: 8px 16px;
  font-size: 1rem;
  text-transform: uppercase;
  letter-spacing: 1px;
  border-radius: 8px;
  border: none;
  transition: background-color 200ms, color 200ms;
  cursor: pointer;
}

.primary {
  background-color: hsl(245deg 100% 60%);
  color: white;
}
.secondary {
  background-color: transparent;
  color: hsl(15deg 10% 30%);
}

.primary:hover {
  background-color: hsl(245deg 75% 50%);
}

.secondary:hover {
  background-color: hsl(15deg 60% 90% / 0.5);
  color: black;
}

.wrapper:disabled {
  opacity: 0.5;
  cursor: not-allowed;
}
```

We have a new bit of state, `hasAgreed`, and we need to use that data to power our “Confirm” button. This reveals an important truth about props: **they're the tunnels that allow data to flow through our application.**

- React state is used to track data that changes over time. In this case, `hasAgreed` can flip between true/false as the user toggles the checkbox.
- By default, state is only made available to the *current* component. Our `Button.js` component isn't privy to the `hasAgreed` state variable. State isn't global.
- Props allow us to funnel data from one component to another. You can  think of it as an interstate highway system for our data, or like a  network of tunnels that data can flow through.
- We can pass static values (strings, numbers, boolean values) through props, *or* we can pass dynamic state variables. It doesn't matter to React.