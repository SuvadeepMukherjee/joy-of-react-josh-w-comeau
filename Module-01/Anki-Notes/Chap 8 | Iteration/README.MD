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

**Q2**:Is key a prop ? 

**Solution**:

Key looks like a prop but its a special thing React uses to identify an element 

**Q3**:What are the 2 Key Rules? 

**Solution**:

- Top Level Element

- When using fragments, its sometimes required to switch to the long-form `React.Fragmemt` so that we can apply the key

**Q4**:Do keys need to be globally unique across the entire application ?

**Solution**:

Many developers believe that keys has to be *globally* unique, across the entire application, but this is a misconception. Keys only have to be unique within their array.Each `.map()` call produces a separate array, and so there's no problem. 👍
