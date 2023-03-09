---
layout: default
title: List comprehension
parent: Haskell
grand_parent: Notes
---

## List comprehension
Relevant content from Chapter 5 of [Programming in Haskell 2nd Edition](https://www.cambridge.org/us/academic/subjects/computer-science/programming-languages-and-applied-logic/programming-haskell-2nd-edition)

### Basic concepts
In Haskell a list comprehension notation can be used to construct new lists from existing lists. For example:

```haskell
> [x^2 | x <- [1..5]]
[1,4,9,16,25]
```

The symbol **|** is read as *such that*, **<-** is read as drawn from, and the expression `x <- [1..5]` is called a *generator*. A list comprehension can have more than one generator, with successive generators being separated by commas.

```haskell
> [(x,y) | x <- [1,2,3], y <- [4,5]]
[(1,4), (1,5), (2,4), (2,5), (3,4), (3,5)]
```

Changing the order of the two generators in the previous example produces the same set of pairs, but arranged in a different order.
```haskell
> [(x,y) | y <- [4,5], x <- [1,2,3]]
[(1,4), (2,4), (3,4), (1,5), (2,5), (3,5)]
```
This behavior can be understood by thinking of later generators as being more deeply nested, and hence changing the values of their variables more frequently than earlier generators.

#### Concat
Concatenates a list of lists. It can be defined by using one generator to select each list in turn, and another to select each element form each list:

```haskell
concat :: [[a]] -> [a]
concat xss = [x | xs <- xss, x <- xs]
```
