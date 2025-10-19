# Chap 2 | The useId Hook

Anki ➡️ 02-The-useId-Hook

**Q1**:A core idea with React is that components are *reusable*. We should be able to render multiple instances of just about any component .However, certain aspects of the web platform aren't built with reusability in mind. For example, `id` attributes need to be globally unique! How can we create reusable components without accidentally violating this rule?

**Solution**:

We can use the `useId` hook

```js
function LoginForm() {
  const id = React.useId();
}
```

- Each instance of this component will receive a different value. For example, if we render 2 instances, we'll notice that the `id` variable is equal to `:r0:` in the first one and `:r1:` in the second.
- **React hooks “hook into” the component instance.** And so this ID lives on the instance, and can be plucked out during render.
- This means that the value **persists across renders.** `id` will always be equal to `:r0:`, in that first instance, on every single render.

**Q2**: Fill in the blanks 

```js
function LoginForm() {
  const id = React.useId();
}
```

- Each instance of this component will receive a _________
- React hooks _________ the component instance.
- This means that the value __________

**Solution**:

- Each instance of this component will receive a different value. For example, if we render 2 instances, we'll notice that the `id` variable is equal to `:r0:` in the first one and `:r1:` in the second.
- **React hooks “hook into” the component instance.** And so this ID lives on the instance, and can be plucked out during render.
- This means that the value **persists across renders.** `id` will always be equal to `:r0:`, in that first instance, on every single render.

**Q3**:Does the `useId` hook produce the same value across server and client renders ? 

**Solution**:

The `useid` hook produce the same value across server and client renders .This is a very special property and something that would be very difficult to reproduce without a special React-provided solution  

