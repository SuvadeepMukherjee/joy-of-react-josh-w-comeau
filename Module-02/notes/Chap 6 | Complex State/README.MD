# Chap 6 | Complex State

## Complex State

So far, we've seen how we can store numbers and strings in React state.  In many real-world cases, however, our state will be in a more complex  shape, like an object or an array.

React doesn't care what type our state is. We can store numbers or strings or arrays or objects. We can even store *functions* in state if we want!(We *can*, but in general, we shouldn't. )

**But there's a catch:** React state changes have to be *immutable*. We should never mutate arrays or objects held in state 

```jsx
const [colors, setColors] = React.useState(['#FFD500', '#FF0040']);
```

Lets look at some code : 

```jsx
function App() {
  const [colors, setColors] = React.useState([
    '#FFD500',
    '#FF0040',
  ]);

  const colorStops = colors.join(', ');
  const backgroundImage = `linear-gradient(${colorStops})`;

  return (
    <>
      <div
        className="gradient-preview"
        style={{
          backgroundImage,
        }}
      />

      <form>
        {colors.map((color, index) => {
          const colorId = `color-${index}`;

          return (
            <div key={colorId} className="color-row">
              <label htmlFor={colorId}>
                Color {index + 1}:
              </label>
              <input
                id={colorId}
                type="color"
                value={color}
                onChange={event => {
                  colors[index] = event.target.value;

                  setColors(colors);
                }}
              />
            </div>
          );
        })}
      </form>
    </>
  );
}
```

The problem is that we're mutating the `colors` array. 

When we call `setColors`, we need to provide a *brand new* array. React needs to know that the array / object is new then only it will render . If we mutate the original array/object react will think we didn't change anything and will not render anything  

The solution to this problem is to update our `onchange` function so that we're always generating a new array.

```jsx
<input
  id={colorId}
  type="color"
  value={color}
  onChange={event => {
    const nextColors = [...colors];
    nextColors[index] = event.target.value;

    setColors(nextColors);
  }}
/>
```



The order of operations are the following ➡️

1.  Create a new array
2. Modify that new array
3. Set the new array into state

Note ➡️We must strictly follow this order

## But isn't this inefficient ? 

isn't it inefficient to be creating brand-new arrays on every single change ? 

When it comes to React state , there is no choice , we need to produce a new array whenever the state changes. Fortunately , though this isn't an issue.Cloning an array is an incredibly quick operation.