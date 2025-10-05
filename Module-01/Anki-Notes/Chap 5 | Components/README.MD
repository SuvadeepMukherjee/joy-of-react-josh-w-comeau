# Chap 5 | Components

Anki ➡️ 05-components

**Q1**:What do you mean by the statement "React is a component based framework " ? 

**Solution**:

- React is a component based framework
- A component is a bundle of markup, styles and logic that controls everything about a specific part of the user interface
- In React components are the main mechanism of reuse

------

**Q2**: What is the Big Component Rule ? 

**Solution**:

React components need to start with a capital letter .This is how the JSX compiler can tell whether we are trying to render a built-in HTML tag or a custom React component  . We have to enforce this rule because even if we have a new rule say Components can't share names with HTML tags but this will not work because every now and then we get a new HTML tag! . The list of HTML tags isn't set in stone. 

The convention is to use PascalCase for all React component names