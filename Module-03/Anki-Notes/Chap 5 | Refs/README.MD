# Chap 5 | Refs

Anki ➡️ 05-refs

**Q1**:Drawing a pink rectangle with canvas :

```js 
const canvas = document.querySelector('canvas');
const ctx = canvas.getContext('2d');

// Draw a 200 × 100 pink rectangle:
ctx.fillRect(0, 0, 200, 100);
```

 Why can’t we directly use `document.querySelector()` to work with a `<canvas>` in React, and what’s the correct React way to access it?



**Solution**:

To access the DOM nodes we use  the `ref` attribute!

```jsx
<canvas
  width={200}
  height={200}
  ref={function (canvas) {
    console.log(canvas); // <canvas> DOM node
  }}
/>
```

Whenever React renders this element, it will invoke the `ref` function and provide the underlying DOM node.

**Q2**: Direct DOM queries (`document.querySelector`) are discouraged in React because React manages the DOM and bypassing it can lead to inconstencies instead we use the `ref` attribute which gives a reference to the actual DOM node when React renders the element. Here the `canvas` variable is assigned inside the ref callback . The draw function is then called on the button click , using the `canvas` reference.This function can access the canvas's 2D context and draw shapes, lines or other graphics  

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

There is one issue with this code : the `ref` function will be invoked on every single render , meaning we'll constantly be looking up this DOM node 

It would be better if we could do this work once , when the component first renders . We could then pass the canvas reference through each subsequent render . 

What is the conventional way to work with DOM node reference in React

**Solution**:

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
    			// ref ={canvasRef} is a more performant way of doing the following
    			// canvasRef.current = ref 
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

**Q3**:The ref object created by the `useRef` hook can be thought of as a box . What we can store in this box ? 

**Solution**:

The ref object created by the `useRef` hook should be thought of as a box. We can store whatever we want in  this box: DOM nodes, numbers, arrays, objects, functions, etc.

That said, the *primary* use case for refs is to store DOM nodes. It's pretty rare for me to need to store anything else in a ref.