# Chap 3 | Statements vs Expressions

Anki ➡️ 03-statements-vs-expressions

**Q1**: What is the difference between a statement and a expression ? 

**Solution**:

Expression ➡️ 

- A bit of JS code that produces a value , `[1,2,3].pop()`
- Expressions can contain expressions , `(5+1)*2`

Statement ➡️

- Each statement is an expression to the computer to do a particular thing . Statements often have slots for expressions .We can put any expression we like in these slots 

  ```js
  let hi = 5;
  throw new Error("Something exploded")
  ```

A Handy Trick ➡️

- Want to know whether a chunk of JS is an expression or statement ? Try to log it out .If it runs , the code is an expression . If you get an error its a statement(or possibly invalid JS)

Note ➡️

expressions cant exist on their own .They are always  part of a statement