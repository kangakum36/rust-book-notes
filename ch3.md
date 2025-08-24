# Chapter 3: Common Programming Concepts

## Variables and Mutability
- `mut` -> value can change
- `const` -> value cannot change, can't use mut
- **shadowing** is diff from mut, can't change type of a mut variable

## Data Types
- integer wraparound in release mode, else panic
- **tuples** have structured binding like syntax similar to cpp/python
- rust does runtime index checks on arrays

## Functions and Control Flow
### Statements vs Expressions
- **statement** = series of instructions that do not return a value
- **expression** = evaluates to a resultant value
- functions can be statements or expressions
- last line of an expression ends without a semicolon - is return value

### Conditionals and Loops
- can use `if` in a `let` statement if the branches are expressions
- can use **loop labels** to disambiguate between loops, so can properly break