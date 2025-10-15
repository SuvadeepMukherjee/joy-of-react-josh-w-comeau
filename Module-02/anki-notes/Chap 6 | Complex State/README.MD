# Chap 6 | Complex State

Anki ➡️ 06-complex-state

**Q1**: Is the above statement true ? 

"React state changes have to be immutable "

**Solution**:

React state changes have to be *immutable*. We should never mutate arrays or objects held in state 

```jsx
const [colors, setColors] = React.useState(['#FFD500', '#FF0040']);
```

When we call `setColors`, we need to provide a *brand new* array

