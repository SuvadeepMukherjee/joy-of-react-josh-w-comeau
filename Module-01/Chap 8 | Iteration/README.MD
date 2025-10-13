# Chap 8 | Iteration

## Mapping Over Data

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

```js
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

*Solution Code*

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

## JSX inside JS inside JSX

```jsx
<ul>
  {items.map(item => (
    <li>{item}</li>
  ))}
</ul>
```

On the second line, we use curly brackets to add some "vanilla JS" to
 our JSX. But then we're using JSX inside those curly brackets! Some 
developers are caught off guard by the fact that this is "legal".

The JSX compiler is able to resolve "nested" JSX calls without issue. The end result is something like this:

```js
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

## Keys

When we give React an array of elements , we also need to help React out by uniquely identifying each element 

```js
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
    </ul>
  );
}
```

The purpose of a key is to uniquely identify each React element 

- React doesn't actually know specifically what happens when we change 
  data. All React sees is the before/after. React has to figure out how to
   change the DOM to match this new snapshot.

- There are an infinite number of possibilities, and React doesn't 
  actually know which one to take. It could delete and add new items, or 
  it could edit existing ones.

- Different approaches have different implications from a performance perspective. There are also edge-case bugs that can arise.

- Keys exist to show React the route. By uniquely identifying each item 
  across all snapshots, React can figure out exactly what to do, ensuring 
  the best performance and no edge-case issues.

## Is Key a prop

Lets look at the following JSX:

```js
const element = (
  <ContactCard
    key="sunita-abc123"
    name="Sunita Kumar"
    job="Electrical Engineer"
    email="sunita.kumar@acme.co"
  />
);
```

At first glance, it *seems* like we've given this component 4 props: `key`, `name`, `job`, and `email`.

But, if we add a `console.log` to this `ContactCard` component, we'll notice that something's missing:

```js
function ContactCard({ key, name, job, email }) {
  console.log(key); // undefined
  console.log(name); // 'Sunita Kumar'
  console.log(job); // 'Electrical Engineer'
  console.log(email); // 'sunita.kumar@acme.co'

  return (
    <li className="contact-card">
      {/* ✂️ Removed for brevity */}
    </li>
  );
}
```

We've specified 4 props, but only 3 have come through! `key` has not been provided.

**Here's the deal:** there are a small number of “reserved words” in React. `key` is one of these special terms. When we apply a `key` to a React element, we're not *actually* setting it as a prop.

Let's dig into this. First, let's take a look at this in plain JavaScript, no JSX:

```js
const element = React.createElement(
  ContactCard,
  {
    key: 'sunita-abc123',
    name: 'Sunita Kumar',
    job: 'Electrical Engineer',
    email: 'sunita.kumar@acme.co',
  }
);
```

Hmmm… So far, `key` still looks like a prop. Let's keep going.

This code shows that we're calling the `React.createElement` function. As the name implies, this function creates a React element. 
If we were to execute this code, we'd be left with something like this:

```js
const element = {
  type: ContactCard,
  key: 'sunita-abc123',
  props: {
    name: 'Sunita Kumar',
    job: 'Electrical Engineer',
    email: 'sunita.kumar@acme.co',
  }
}
```

The `React.createElement()` function has taken our data and used it to produce a React element, and that element has `key` as a top-level property!

React elements are JavaScript objects that *describe* a thing that React needs to create. In this case, the element describes a `ContactCard` component that needs to be rendered.

*Keys identify a particular React element.* It's a property of the element itself, not something that needs to be passed along to the component!

`key` *looks* like a prop, but it's a special thing that React uses to identify React elements.

#### Key Rules

- Top Level Element

- When using fragments, its sometimes required to switch to the long-form `React.Fragmemt` so that we can apply the key

- Many developers believe that keys has to be *globally* unique, across the entire application, but this is a misconception. Keys only have to be unique within their array.Each `.map()` call produces a separate array, and so there's no problem. 👍
