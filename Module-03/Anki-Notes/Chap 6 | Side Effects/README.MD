# Chap 6 | Side Effects

Anki ➡️ 06-side-effects

**Q1**:What are Side Effects ? 

**Solution**:

- **Core React Loop** ➡️ We give React a chunk of JSX and it produces the resulting DOM .When the JSX changes (eg because the state or props change), React will update the DOM to match, always keeping it in sync .This is React's main job , its primary purpose
- Things that fall outside of this main job like *updating the document's title* , *making network requests* , *interacting with local storage* are known as side effects 

**Q2**:Why React team gave us the `useEffect` hook ? 

**Solution**:

- Core React loop ➡️ We give React a chunk of JSX and it produces the resulting DOM .When the JSX changes (eg because the state or props change), React will update the DOM to match, always keeping it in sync .This is React's main job , its primary purpose
- Things that fall outside of this main job like updating the document's title , making network requests , interacting with local storage are known as side effects 
- React gives us a specific tool for managing side effects : the useEffect hook

**Q3**:Explain the useEffect Hook ? 

**Solution**:

**No dependency array** ➡️

`useEffect(() => { … })`

- Runs after every render , both initial and subsequent renders

**Empty dependency array**

`useEffect(() => { … }, [])`

- Runs only once , after the first render (component mount)

**Dependency array with variables**

`useEffect(() => { … }, [var1, var2])`

- Runs **after the first render**
- And then **any time any of the variables in the array change**.



