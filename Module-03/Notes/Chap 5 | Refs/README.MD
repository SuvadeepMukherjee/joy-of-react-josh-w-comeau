# Chap 5 | Refs

## Refs

So, typically as web developers, we build user interfaces out of a standard set of DOM primitives: divs, buttons, forms, etc.

But the web also comes with a totally different way to draw UI: *HTML Canvas*.

If you're not familiar, HTML Canvas offers a “Microsoft Paint” style ability to create graphics by drawing shapes.

In order to work with HTML Canvas, we start by rendering a `<canvas>` tag, and then running a bunch of commands on it using JavaScript. It might look like this:

```js
const canvas = document.querySelector('canvas');
const ctx = canvas.getContext('2d');

// Draw a 200 × 100 pink rectangle:
ctx.fillRect(0, 0, 200, 100);
```

`ctx` is the drawing surface. We can choose to draw either in 2D or 3D (via webgl).

**Here's the question:** How might we work with this element in React?

When we write JSX, we're not actually writing HTML. We create JS, and React turns that JS into real DOM.

That distance is normally a wonderful thing, since it means React  manages all the hard stuff for us. But sometimes, it gets in the way,  when we actually need to get access to the DOM nodes. This is the case  when it comes to using the `<canvas>` tag.

Now, we *could* use `document.querySelector` to grab a reference to the element after it's rendered, but this is a bad  practice in React. We don't want to “reach around” React's abstraction,  we want to work with it!

React provides an escape hatch for exactly this sort of situation: The `ref` attribute!

```js
<canvas
  width={200}
  height={200}
  ref={function (canvas) {
    console.log(canvas); // <canvas> DOM node
  }}
/>
```

Whenever React renders this element, it will invoke the `ref` function and provide the underlying DOM node.

In the example at hand, I draw a Pokéball using a set of canvas commands. If I rewrite my `draw` function to take the canvas reference as an argument, I can call the function like so:

```js
<canvas
  width={200}
  height={200}
  ref={function (canvas) {
    draw(canvas);
  }}
/>
```

This  approach works, but it's not the user experience I want. Instead, I want users to click the "Draw!" button to trigger the animation.

In order to access this canvas ref within the button's `onClick` handler, I need to hold it in a variable:

```js
function ArtGallery() {
  let canvas;

  return (
    <main>
      <div className="canvas-wrapper">
        <canvas
          ref={function (ref) {
            canvas = ref;
          }}
          width={200}
          height={200}
        />
      </div>
      <button
        onClick={() => {
          draw(canvas);
        }}
      >
        Draw!
      </button>
    </main>
  );
}

function draw(canvas) {
  // Code omitted
}
```

This works *surprisingly* well, but it does suffer from a slight performance issue: the `ref` function will be invoked on *every single render*, meaning we'll constantly be looking up this DOM node.

It would be better if we could do this work once, when the component first renders. We could then pass the canvas reference through each  subsequent render.

We can do this with a hook we haven't encountered yet, `useRef`:

```js
function ArtGallery() {
  // 1. Create a “ref”, a box that holds a value.
  const canvasRef = React.useRef(); // { current: undefined }

  return (
    <main>
      <div className="canvas-wrapper">
        <canvas
          // 2. Capture a reference to the <canvas> tag,
          // and put it in the “canvasRef” box.
          //
          // { current: <canvas> }
          ref={canvasRef}
          width={200}
          height={200}
        />
      </div>
      <button
        onClick={() => {
          // 3. Pluck the <canvas> tag from the box,
          // and pass it onto our `draw` function.
          draw(canvasRef.current);
        }}
      >
        Draw!
      </button>
    </main>
  );
}
```

When we call `React.useRef`, we're given an object with a `current` property.

If we pass an object with this shape to the `ref` attribute, React will mutate this object, setting `current` equal to the canvas reference.

This only runs when the component first renders, leading to improved performance.

This is the conventional way to work with DOM node references in React!

Final Code

```js
import React from 'react';

function ArtGallery() {
  // 1. Create a “ref”, a box that holds a value.
  const canvasRef = React.useRef(); // { current: undefined }

  return (
    <main>
      <div className="canvas-wrapper">
        <canvas
          // 2. Capture a reference to the <canvas> tag,
          // and put it in the “canvasRef” box.
          //
          // { current: <canvas> }
          ref={canvasRef}
          width={200}
          height={200}
        />
      </div>
<button
        onClick={() => {
          // 3. Pluck the <canvas> tag from the box,
          // and pass it onto our `draw` function.
          draw(canvasRef.current);
        }}
      >
        Draw!
      </button>
    </main>
  );
}
function draw(canvas) {
  const ctx = canvas.getContext('2d');

  ctx.clearRect(0, 0, 200, 200);

  ctx.beginPath();
  ctx.rect(30, 90, 140, 20);
  ctx.fillStyle = 'black';
  ctx.fill();
  ctx.closePath();

  ctx.beginPath();
  ctx.arc(100, 97, 75, 1 * Math.PI, 2 * Math.PI);
  ctx.fillStyle = 'tomato';
  ctx.fill();
  ctx.closePath();

  ctx.beginPath();
  ctx.arc(100, 103, 75, 0, 1 * Math.PI);
  ctx.fillStyle = 'white';
  ctx.fill();
  ctx.closePath();
  
  ctx.beginPath();
  ctx.arc(100, 100, 25, 0, 2 * Math.PI);
  ctx.fillStyle = 'black';
  ctx.fill();
  ctx.closePath();
  ctx.beginPath();
  ctx.arc(100, 100, 19, 0, 2 * Math.PI);
  ctx.fillStyle = 'white';
  ctx.fill();
  ctx.closePath();
}
export default ArtGallery;
```

Note ➡️

Not just for DOM nodes!  

The ref object created by the `useRef` hook should be thought of as a box. We can store whatever we want in  this box: DOM nodes, numbers, arrays, objects, functions, etc.

That said, the *primary* use case for refs is to store DOM nodes. It's pretty rare for me to need to store anything else in a ref.