# Chap 5 | Props Vs State

Anki ➡️ 05-props-vs-state

**Q1**:What is the difference between props and state ? 

**Solution**:

- React state is used to track data that changes over time. 
- By default, state is only made available to the *current* component
- Props allow us to funnel data from one component to another. We can  think of it as an interstate highway system for our data, or like a  network of tunnels that data can flow through.
- We can pass static values (strings, numbers, boolean values) through props, *or* we can pass dynamic state variables. It doesn't matter to React.