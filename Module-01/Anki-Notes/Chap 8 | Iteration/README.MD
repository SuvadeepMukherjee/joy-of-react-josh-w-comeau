# Chap 8 | Iteration

Anki => 08-iteration

**Q1**: Map over the data array

App.js 

```js
import ContactCard from './ContactCard';

const data = [
  {
    id: 'sunita-abc123',
    name: 'Sunita Kumar',
    job: 'Electrical Engineer',
    email: 'sunita.kumar@acme.co',
  },
  {
    id: 'henderson-def456',
    name: 'Henderson G. Sterling II',
    job: 'Receptionist',
    email: 'henderson-the-second@acme.co',
  },
  {
    id: 'aio-ghi789',
    name: 'Aoi Kobayashi',
    job: 'President',
    email: 'kobayashi.aoi@acme.co',
  },
]


function App() {
  return (
    <ul>
      <ContactCard
        name={"Name goes here"}
        job={"Job goes here"}
        email={"Email goes here"}
      />
    </ul>
  );
}

export default App;
```

ContactCard.js

```jsx
function ContactCard({ name, job, email }) {
  return (
    <li className="contact-card">
      <h2>{name}</h2>
      <dl>
        <dt>Job</dt>
        <dd>{job}</dd>
        <dt>Email</dt>
        <dd>{email}</dd>
      </dl>
    </li>
  );
}

export default ContactCard;
```

**Solution**:

```js
import ContactCard from './ContactCard';

const data = [
  {
    id: 'sunita-abc123',
    name: 'Sunita Kumar',
    job: 'Electrical Engineer',
    email: 'sunita.kumar@acme.co',
  },
  {
    id: 'henderson-def456',
    name: 'Henderson G. Sterling II',
    job: 'Receptionist',
    email: 'henderson-the-second@acme.co',
  },
  {
    id: 'aio-ghi789',
    name: 'Aoi Kobayashi',
    job: 'President',
    email: 'kobayashi.aoi@acme.co',
  },
];

function App() {
  return (
    <ul>
      {data.map((contact) => (
        <ContactCard
    			key ={contact.id}
          id ={contact.id}
          name={contact.name}
          job={contact.job}
          email={contact.email}
        />
      ))}
    </ul>
  );
}

export default App;
```

Note ➡️ We can render an array inside the JSX,React will unpack it for us and render each item for us

Note ➡️ 

When iterating in React, it's not uncommon to wind up with structures like this:

```jsx
<ul>
  {items.map(item => (
    <li>{item}</li>
  ))}
</ul>
```

On the second line, we use curly brackets to add some "vanilla JS" to our JSX. But then we're using JSX inside those curly brackets! Some  developers are caught off guard by the fact that this is "legal".

The JSX compiler is able to resolve "nested" JSX calls without issue. The end result is something like this:

```jsx
React.createElement(
  'ul',
  {},
  items.map(item => (
    React.createElement(
      'li',
      {},
      item
    )
  ))
);
```

**Q**:In React if we dont give a unique key prop to each child in a list we get the following error 

![key-error](../../assets/key-error.png)

Why do we get this error ? 

**Solution**:

- React doesn't actually know specifically what happens when we change  data. All React sees is the before/after. React has to figure out how to change the DOM to match this new snapshot.
- There are an infinite number of possibilities, and React doesn't  actually know which one to take. It could delete and add new items, or  it could edit existing ones.
- Different approaches have different implications from a performance perspective. There are also edge-case bugs that can arise.
- Keys exist to show React the route. By uniquely identifying each item  across all snapshots, React can figure out exactly what to do, ensuring  the best performance and no edge-case issues.



**Q2**:Is key a prop ? 

**Solution**:

Key looks like a prop but its a special thing React uses to identify an element . *Keys identify a particular React element.* It's a property of the element itself, not something that needs to be passed along to the component!

**Q3**:What are the 2 Key Rules? 

**Solution**:

`key` needs to be applied to the very *top-level element* within the `.map()` call. 

This is incorrect 

```jsx
function NavigationLinks({ links }) {
  return (
    <ul>
      {links.map(item => (
        <li>
          <a
            key={item.id}
            href={item.href}
          >
            {item.text}
          </a>
        </li>
      ))}
    </ul>
  );
}
```

From React's perspective, it has a group of `<li>` React elements, and it doesn't see any unique identifiers on them. It doesn't "dig in" and look for keys on children elements.

**Here's how to fix it:**

```jsx
function NavigationLinks({ links }) {
  return (
    <ul>
      {links.map(item => (
        <li key={item.id}>
          <a href={item.href}>
            {item.text}
          </a>
        </li>
      ))}
    </ul>
  );
}
```

When using fragments, its sometimes required to switch to the long-form `React.Fragmemt` so that we can apply the key

```jsx
// 🚫 Missing key:
function Thing({ data }) {
  return (
    data.map(item => (
      <>
        <p>{item.content}</p>
        <button>Cancel</button>
      </>
    ))
  );
}

// ✅ Fixed!
function Thing({ data }) {
  return (
    data.map(item => (
      <React.Fragment key={item.id}>
        <p>{item.content}</p>
        <button>Cancel</button>
      </React.Fragment>
    ))
  );
}
```



**Q4**:Do keys need to be globally unique across the entire application ?

**Solution**:

Many developers believe that keys has to be *globally* unique, across the entire application, but this is a misconception. Keys only have to be unique within their array.Each `.map()` call produces a separate array, and so there's no problem. 👍

Totally valid ➡️

```jsx
function App() {
  return (
    <ul>
      {data.map(contact => (
        <ContactCard
          key={contact.id}
          name={contact.name}
          job={contact.job}
          email={contact.email}
        />
      ))}
      {data.map(contact => (
        <ContactCard
          key={contact.id}
          name={contact.name}
          job={contact.job}
          email={contact.email}
        />
      ))}
    </ul>
  );
}
```

