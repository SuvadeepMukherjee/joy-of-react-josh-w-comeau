# Chap 11 | Styling in React

## Styling in React

---



- React is *unopinionated* when it comes to styling. As a result, there are dozens of options out there when it comes to pairing CSS with React.

- We could write CSS the traditional way . We could keep a bunch of CSS files , and include them with <link> tags in our index.html

- But if we want to have the best experience building with Reacy there are better approaches

- The core idea with components is that each component is a bundle of markup(in JSX) , logic (in JS) ,and styles (in CSS).Our Button componet should own all the styles related to that component ![](../assets/seperation-of-concerns.png)

- When we structure our code this way , working with CSS becomes much simpler 

## Block Element Modifier and React

---

- The traditional way of writing CSS involves writing lots of different 
  selectors to target the same element. This leads to an "arms race", 
  where every selector needs to be more and more aggressive, in order to 
  override existing styles

- This has long been a frustration of writing CSS on large projects, and 
  there have been many proposed solutions. One of the most popular is BEM 
  (Block Element Modifier), a naming methodology.

- BEM does make CSS much easier to work with *if it's followed 100% of the time*.Unfortunately, human nature means that it's very easy to go off the 
  rails, especially when it comes to large projects with many developers. 
  It requires superhuman discipline to follow the methodology.

- When React grew to prominence, the community realized that there was an 
  opportunity to improve things. In the years since, there have been many 
  many different tools released, including styled-components, emotion, 
  vanilla-extract, stitches, etc.

- These tools "bake in" the scoping, generating unique selectors for us automatically.






