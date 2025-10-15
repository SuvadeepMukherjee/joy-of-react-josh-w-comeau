# Chap 3 | The useState Hook

Anki => 03-useState-hook

**Q1**:In React,how do you declare a state variable named `count` initialized to `0` and display it inside a button that increments the value by 1 each time it's clicked ? 

**Solution**:

```jsx
import React from 'react';

function Counter() {
  const [count, setCount] = React.useState(0);

  return (
    <button onClick={() => setCount(count + 1)}>
      Value: {count}
    </button>
  );
}

export default Counter;
```

**Q2**:What is initializer function in useState and why it is useful ? 

**Solution**:

React state variables can be given an initial value:

```jsx
const [count, setCount] = React.useState(1);
console.log(count); // 1
```

We can also supply a *function*. React will call this function on the very first render to calculate the initial value:

```jsx
const [count, setCount] = React.useState(() => {
 return 1 + 1;
});

console.log(count); // 2
```

This is sometimes called an *initializer function*.It can occasionally be useful if we need to do an expensive operation to calculate the initial value. For example, reading from Local Storage:

```jsx
const [count, setCount] = React.useState(() => {
 return window.localStorage.getItem('saved-count');
});
```

The benefit here is that we're only doing the expensive work (reading from 
Local Storage) once, on the initial render, rather than doing it on every single render.

**Q3**:What exactly is the difference between these 2 forms ? 

```jsx
// Form 1:
const [count, setCount] = React.useState(
  window.localStorage.getItem('saved-count')
);

// Form 2:
const [count, setCount] = React.useState(() => {
  return window.localStorage.getItem('saved-count');
});
```

**Solution:**

```jsx
function Counter() {
  const [count, setCount] = React.useState(() => {
    return window.localStorage.getItem('saved-count');
  });
}
```

Whenever we render the `Counter` component, this function will be called, and all of the code inside will run. We'll call `window.localStorage` on every single render, and pass the value into `React.useState`

Now, let's consider the secondary form:

```jsx
function Counter() {
  const [count, setCount] = React.useState(() => {
    return window.localStorage.getItem('saved-count');
  });
}
```

In this case, we're *creating a function*:

```jsx
() => {
  return window.localStorage.getItem('saved-count');
}
```

This *function* is being passed into `React.useState()`. And so it's up to React to decide what to do with it.

On the very first render, React will call this function to calculate the initial value. On *subsequent* renders, however, React ignores the function. The initial value has 
already been calculated, and so there's no reason to call the function 
again.

**Q4:** What is reconcilation ? 

**Solution:** Reconcilation is a process where React using fancy optimized algorithms figures out the  changes  between 2 snapshots (snapshot is like a photo that captures what things were like at a moment in time)

**Q5:** Explain  the core react loop 

**Solution:**

![](../../assets/core-react-loop.png)

Mount:**

When we render the component for the first time , there is no previous snapshot to compare to.And so , React will create all of the necessary DOM nodes from scratcg , and inject them into the page.

**Trigger**:

Eventually , something happens that triggers a state change, invokig the "setX" function (eg. setCount).We're telling React that the value of a state variable has just been updated

**Render:**

Because the state changed, we need to generate a new description of the UI! React will *reconcile* it, comparing it to the previous snapshot , and figuring out what needs to happen in order for the DOM to match this latest snapshot

**Commit:**

If any DOM updates are required,React will perform those mutations(eg Changing the text content of a DOM node , creating new nodes, deleting removed nodes , etc)

Once the changes have been committed ,React goes idle , and waits for the next trigger , the next state change.

**Q6:** Explain the difference between re-render and re-paint

**Solution**:

- A *re-render* is a React process where it figures out what needs to change (AKA. “reconciliation”, the spot-the-differences game).

- *If* something has changed between the two snapshots(snapshot => Represntation what the UI will look like), React will 
  “commit” those changes by editing the DOM, so that it matches the latest
   snapshot.

- Whenever a DOM node is edited, the browser will *re-paint*, re-drawing the relevant pixels so that the user sees the correct UI.

- **Not all re-renders require re-paints!** If nothing has changed between snapshots(Representation of what the UI would look like ?), React won't edit any DOM nodes, and nothing will be re-painted.

**Q7**: Does all re-renders require repaints ? 

**Solution:**

- A *re-render* is a React process where it figures out what needs to change (AKA. “reconciliation”, the spot-the-differences game).

- *If* something has changed between the two snapshots(snapshot => Represntation what the UI will look like), React will 
  “commit” those changes by editing the DOM, so that it matches the latest
   snapshot.

- Whenever a DOM node is edited, the browser will *re-paint*, re-drawing the relevant pixels so that the user sees the correct UI.

- **Not all re-renders require re-paints!** If nothing has changed between snapshots, React won't edit any DOM nodes, and nothing will be re-painted.

**Q8**:What value would we expect to see in the developer console when the user clicks the button for the first time ? 

```jsx
function App() {
  const [count, setCount] = React.useState(0);

  return (
    <>
      <p>
        You've clicked {count} times.
      </p>
      <button
        onClick={() => {
          setCount(count + 1);

          console.log(count)
        }}
      >
        Click me!
      </button>
    </>
  );
}
```

**Solution:**

State setters aren't immediate. When we call `setCount`, we tell React that we'd like to request a change to a state variable. React doesn't immediately drop 
everything; it waits until the current operation is completed (processing the click), and then it updates the value and triggers a re-render.**Updating a state variable is asynchronous.** It affects what the state will be *for the next render*. It's a scheduled update.

**Q9:** Why is updating a state variable asynchronous ? 

**Solution:**

Lets take a look at some code :

```jsx
import React from 'react';

function App() {
  const [user, setUser] = React.useState({ name: 'Alyssa' });
  const [status, setStatus] = React.useState('ready');
  const [confirmationMessage, setConfirmationMessage] = React.useState();

  if (!user) {
    return <p>{confirmationMessage}</p>;
  }

  return (
    <button
      onClick={() => {
        setUser(null);
        setStatus('initial');
        setConfirmationMessage("You have been logged out.");
      }}
    >
      Log Out
    </button>
  );
}

export default App;
```

When the user clicks the big “Log Out” button, here's what happens:

1. The `setUser` function is called. React makes a note of this, assigning itself a metaphorical Jira? ticket. `user` will be changed to `null`.

2. Then, on the next line, `setStatus` is called. React *edits* the Jira ticket, noting that on the next render, *two* state variables have to change.

3. Finally, `setConfirmationMessage` is called. React edits the ticket again. It now knows the new value for all 3 state variables.

Once this `onClick` handler wraps up, React springs into action, performing all the steps we learned about in the Core React Loop lesson. It invokes the `App` function, `user` is initialized to `null`, and a paragraph is returned. React deletes the `<button>` DOM node, produces a new `<p>` DOM node, and the re-render is completed.

**Now, let's imagine if state updates were synchronous.**

When the user clicks the button, the click handler would be called, and `setUser(null)` would run. React would *immediately* do the re-render, performing all of the steps we talked about. It calls the `App` function, gets the result, destroys the button, creates a paragraph.

Only after all that, it resumes what it was doing in the `onClick` handler. And, immediately, another re-render is triggered for the `status` variable. And then, after all that, a third re-render for `confirmationMessage`.

We'dbe forcing react to do 3x the amount of work, which would take 3x as 
long. So, it would be a performance liability. But it would also lead to
 inconsistent / broken UI!For example: if we did a re-render after the first `setUser` call, `user` would be `null`, but `confirmationMessage` would still be undefined, since we haven't gotten there yet! As a result, we'd wind up with the following JSX:

{undefined}

Because state updates are asynchronous, they can be *batched*.
 React schedules the update, to take place as soon as the current work 
is completed (in practice, this is usually within a millisecond or two, 
so it feels completely instantaneous).

**Q9**:Bastions and Basilisks Bug 

You're building an in-browser role-playing game, *Bastions and Basilisks*, but you've hit a snag! The wrong values are being shown to the user when their character levels up.

Your mission is to fix the bug.

**Acceptance Criteria:**

- When the “Level up” button is clicked, the `alert` that pops up should show the *updated* values for strength/dexterity/intelligence.

For example, when clicking the button for the first time, “strength” should be displayed as 7, not 6.

```jsx
import React from 'react';

function Character() {
  const [strength, setStrength] = React.useState(6);
  const [dexterity, setDexterity] = React.useState(9);
  const [intelligence, setIntelligence] = React.useState(15);

  function triggerLevelUp() {
   (strength)=> setStrength(strength + 1);
   (dexterity)=> setDexterity(dexterity + 2);
   (intelligence)=> setIntelligence(intelligence + 3);

    window.alert(`
      Congratulations! Your hero now has the following stats:
      Str: ${strength}
      Dex: ${dexterity}
      Int: ${intelligence}
    `);
  }
return (
    <div className="wrapper">
      <img
        alt="8-bit wizard character"
        src="https://sandpack-bundler.vercel.app/img/mage-sprite.gif"
      />
      <button onClick={()=>triggerLevelUp()}>
        Level Up
      </button>
    </div>
  );
}

export default Character;
```

**Solution**:

```jsx
import React from 'react';

function Character() {
  const [strength, setStrength] = React.useState(6);
  const [dexterity, setDexterity] = React.useState(9);
  const [intelligence, setIntelligence] = React.useState(15);

  function triggerLevelUp() {
    const nextStrength = strength + 1;
    const nextDexterity = dexterity + 2;
    const nextIntelligence = intelligence + 3;

    setStrength(nextStrength);
    setDexterity(nextDexterity);
    setIntelligence(nextIntelligence);

    window.alert(`
      Congratulations! Your hero now has the following stats:
      Str: ${nextStrength}
      Dex: ${nextDexterity}
      Int: ${nextIntelligence}
    `);
  }

  return (
    <div className="wrapper">
      <img
        alt="8-bit wizard character"
        src="https://sandpack-bundler.vercel.app/img/mage-sprite.gif"
      />
      <button onClick={triggerLevelUp}>
        Level Up
      </button>
    </div>
  );
}

export default Character;
```

**Q10**:Counter 2.0

The counter has been upgraded, and now supports several functions.

The buttons have already been added to the page, but they don't do anything yet. **Your job is to wire up the buttons.**

**Acceptance Criteria:**

The- button should increase the count by 1.

The- button should increase the count by 10.

The- button should reset the count to 0.

The- button should set the count to a random number between 1 and 100.

The- button should decrease the count by 10.

The button should decrease the count by 1.

```jsx
import React from 'react';
import { ChevronUp, ChevronsUp, ChevronDown, ChevronsDown, RotateCcw, Hash } from 'react-feather'

function Counter() {
  const [count, setCount] = React.useState(0);

  function increaseSlightly(count){
    const nextCount = count+1;
    setCount(nextCount)
  }
return (
    <div className="wrapper">
      <p>
        <span>Current value:</span>
        <span className="count">
          {count}
        </span>
      </p>
      <div className="button-row">
        <button onClick={increaseSlightly}>
          <ChevronUp />
          <span className="visually-hidden">
            Increase slightly
          </span>
        </button>
        <button>
<ChevronsUp />
          <span className="visually-hidden">
            Increase a lot
          </span>
        </button>
        <button>
          <RotateCcw />
          <span className="visually-hidden">
            Reset
          </span>
        </button>
        <button>
          <Hash />
          <span className="visually-hidden">
            Set to random value
          </span>
        </button>
        <button>
          <ChevronsDown />    
<span className="visually-hidden">
            Decrease a lot
          </span>
        </button>
        <button>
          <ChevronDown />
          <span className="visually-hidden">
            Decrease slightly
          </span>
        </button>
      </div>
    </div>
  );
}

export default Counter;
```

**Solution**:

```js
import React from 'react';
import {
  ChevronUp,
  ChevronsUp,
  ChevronDown,
  ChevronsDown,
  RotateCcw,
  Hash,
} from 'react-feather';

const clamp = (val, min = 0, max = 1) => {
  if (min > max) {
    [min, max] = [max, min];
  }

  return Math.max(min, Math.min(max, val));
};

function Counter({ initialVal = 0 }) {
  const [
    count,
    setCount,
  ] = React.useState(initialVal);

  return (
    <div className="wrapper">
      <p>
        <span>Current value:</span>
        <span className="count">
          {count}
        </span>
      </p>
      <div className="button-row">
        <button
          onClick={() =>
            setCount(count + 1)
          }
        >
          <ChevronUp />
          <span className="visually-hidden">
            Increase slightly
          </span>
        </button>
        <button
          onClick={() =>
            setCount(count + 10)
          }
        >
          <ChevronsUp />
          <span className="visually-hidden">
            Increase a lot
          </span>
        </button>
        <button
          onClick={() =>
            setCount(initialVal)
          }
        >
          <RotateCcw />
          <span className="visually-hidden">
            Reset
          </span>
        </button>
        <button
          onClick={() => {
            const nextCount = clamp(
              Math.ceil(
                Math.random() * 100
              ),
              1,
              100
            );
            setCount(nextCount);
          }}
        >
          <Hash />
          <span className="visually-hidden">
            Set to random value
          </span>
        </button>
        <button
          onClick={() =>
            setCount(count - 10)
          }
        >
          <ChevronsDown />
          <span className="visually-hidden">
            Decrease a lot
          </span>
        </button>
        <button
          onClick={() =>
            setCount(count - 1)
          }
        >
          <ChevronDown />
          <span className="visually-hidden">
            Decrease slightly
          </span>
        </button>
      </div>
    </div>
  );
}

export default Counter;
```
