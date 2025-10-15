# Chap 6 | Complex State

Anki ➡️ 06-complex-state

**Q1**: Is the above statement true ? 

"React state changes have to be immutable "

**Solution**:

React state changes have to be *immutable*. We should never mutate arrays or objects held in state 

```jsx
const [colors, setColors] = React.useState(['#FFD500', '#FF0040']);
```

**Q2**: Why React state changes have to be immutable ? 

**Solution**:

React needs to know that the array/object is new then only it will render .If we mutate the original array/object react will think we didn't change anything and will not render anything

**Q3**: What is the order of opertions when we have to change state of arrays/objects ? 

**Solution**:

The order of operations are as follows : 

1. Create a new array/object
2. Modify that new array/object
3. Set the new array into state

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

Note ➡️ We must strictly follow this order 

Note ➡️ When it comes to React state we need to produce a new array/object whenever the state changes .Fortunately , though this isn't an issue because cloning an array is an incredibly quick operation(so it is not inefficient to create brand-new arrays on every single change)