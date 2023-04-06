---
layout: default
title: Interactive Programming
parent: Haskell
nav_order: 9
grand_parent: Notes
---

## Interactive programming
Relevant content from Chapter 10 of [Programming in Haskell 2nd Edition](https://www.cambridge.org/us/academic/subjects/computer-science/programming-languages-and-applied-logic/programming-haskell-2nd-edition)

### The problem

*Interactive programms* require input from the user. This is achieved while still staying in the realm of pure functions with a new type.

### The solution

An *interactive program* is viewed as a pure function that takes the current *state of the world* as its argument, and produces a modified world as its result. The modified world reflects any side effects taht were performed by the pogram during execution.

Additionaly an *interactive program* may return a result in addition to performing side effects.

```haskell
type IO a = World -> (a, World)
```

Expressions of type `IO a` are called *actions*. `IO ()` is the type of *actions* tht return the empty tuple `()` as a dummy result value.


