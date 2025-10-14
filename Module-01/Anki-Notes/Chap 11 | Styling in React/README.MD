# Chap 11 | Styling in React

Anki => 11-styling-in-react

**Q1**:What is React way of styling ? 

**Answer**:

![](../../assets/seperation-of-concerns.png)

The core idea with components is that each componen is a bundle of markup(in JSX), logic (in JS) and styles (in CSS).Our component should own all the styles related to that component

---

**Q2**:What is Block Element Modfier and why is it used ? 

**Solution**:

- The traditional way of writing CSS involves writing lots of different
  selectors to target the same element. This leads to an "arms race",
  where every selector needs to be more and more aggressive, in order to
  override existing styles

- This has long been a frustration of writing CSS on large projects, and
  there have been many proposed solutions. One of the most popular is BEM
  (Block Element Modifier), a naming methodology.

- BEM does make CSS much easier to work with *if it's followed 100% of the time*.

**Q3**: What do styled-components , CSS modules , vanilla-extract,stitches etc do ? 

**Answer**:

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
  vanilla-extract, stitches, CSS Modules etc.

- These tools "bake in" the scoping, generating unique selectors for us automatically.

---

**Q4**:Modern Bundlers like Webpack support importing non-JS files like CSS modules ? (True/False)

**Solution**:

- Yes Modern Bundlers like Webpack support importing non-JS files like CSS modules

---

**Q5**:When our CSS file ends in a `.module.css` and we import it like a JS module what things happen 
