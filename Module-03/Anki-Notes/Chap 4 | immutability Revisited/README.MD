# Chap 4|Immutability Revisited

**Q1**:In React, when we update state like `setUser({ name: 'Ava' })`, how does it affect memory, and what happens to the `items` state variable if it wasn’t changed?

**Solution**:

- React treats state as **immutable**, so calling `setUser({ name: 'Ava' })` creates a **new object** instead of mutating the old `{ name: 'Ivy' }`.

- Both `{ name: 'Ivy' }` and `{ name: 'Ava' }` exist briefly in **different memory locations**, until the old one is cleaned up by the garbage collector.

- Each render creates a **new snapshot** with its own `user` and `items` variables.

- Since `items` **was not changed**, React **reuses the same reference** to the same array in memory across renders.

- This reference behavior lets React efficiently detect what truly changed — critical for **`useEffect`**, **`useMemo`**, and **`useCallback`** to work correctly.

Snapshot ➡️

- The specific values of any props/state at the time that the render occurred
- The React element(s) returned from the component, describing the UI calculated in the render.

**Q2**:What's the difference between snapshots and instances ?

**Solution**:

A component instance is a JavaScript object that is the “source of  truth” for everything related to a particular instance of a component.  It's created when the component is mounted, and it persists until the  component is unmounted.

A snapshot, by contrast, is not a specific JavaScript object. It's a more abstract/metaphorical concept. It refers to the data available at a moment in time.

So, we might say that an *instance* holds the true value of a piece of state, but every time that state *changes*, we create a snapshot that captures the *current value* of that state variable.