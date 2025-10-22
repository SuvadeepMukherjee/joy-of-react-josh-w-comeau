# Chap 10 | Range Utility

## Range Utility

we can use `.map` to iterate over an array of data, to render 1 React element per item.But what if we don't *have* an array?For example, it's common for ratings to use a 5-star system. Let's suppose we want to render 0 to 5 little star icons, depending on the rating.

## range function

`range` is a utility function. It's not part of the JavaScript language (unfortunately), but it *is* a staple of utility libraries like lodash.

```js
// Create an array from 0 (inclusive) to 2 (exclusive):
range(2);
// Produces: [0, 1]

// Create an array from 0 (inclusive) to 5 (exclusive):
range(5);
// Produces: [0, 1, 2, 3, 4]

// Create an array from 2 (inclusive) to 6 (exclusive):
range(2, 6);
// Produces: [2, 3, 4, 5]

// Create an array from 2 to 10, picking every 2nd number
range(2, 10, 2);
// Produces: [2, 4, 6, 8]
```

Essentially, **it's the expression version of a “for” loop statement,** like how `&&` can be an expression version of an “if” statement. This means we can use it in our JSX.

Here's how we'd use it:

```jsx
function StarRating({ rating }) {
  return (
    <div className="star-wrapper">
      {range(rating).map((num) => (
        <img
          key={num}
          alt=""
          className="gold-star"
          src="https://sandpack-bundler.vercel.app/img/gold-star.svg"
        />
      ))}
    </div>
  );
}
```

## Range Function code

```js
const range = (start, end, step = 1) => {
  let output = [];

  if (typeof end === 'undefined') {
    end = start;
    start = 0;
  }

  for (let i = start; i < end; i += step) {
    output.push(i);
  }

  return output;
};
```

## Complete Solution demonstrating this function

App.js => 

```js
import StarRating from './StarRating';

function App() {
  return (
    <>
      <section>
        <h2>Winter's Orbit</h2>
        <p>
          Rating: 5 / 5
        </p>
        <StarRating rating={5} />
      </section>
      <section>
        <h2>Dial D for Deadman</h2>
        <p>
          Rating: 4 / 5
        </p>
        <StarRating rating={4} />
      </section>
      <section>
        <h2>Words And Things</h2>
        <p>
          Rating: 0 / 5
        </p>
        <StarRating rating={0} />
      </section>
      <section>
        <h2>The Galaxy, And The Ground Within</h2>
        <p>
          Rating: 5 / 5
        </p>
        <StarRating rating={5} />
      </section>
    </>
  );
}

export default App;
```

StarRating.js => 

```jsx
function StarRating({ rating }) {
  return (
    <div className="star-wrapper">
      {range(rating).map((num) => (
        <img
          key={num}
          alt=""
          className="gold-star"
          src="https://sandpack-bundler.vercel.app/img/gold-star.svg"
        />
      ))}
    </div>
  );
}

const range = (start, end, step = 1) => {
  let output = [];

  if (typeof end === 'undefined') {
    end = start;
    start = 0;
  }

  for (let i = start; i < end; i += step) {
    output.push(i);
  }

  return output;
};

export default StarRating;
```


