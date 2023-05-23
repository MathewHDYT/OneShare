---
layout: default
title: Reasoning about programs
parent: Haskell
nav_order: 11
grand_parent: Notes
---

## Reasoning about programs
Relevant content from Chapter 16 of [Programming in Haskell 2nd Edition](https://www.cambridge.org/us/academic/subjects/computer-science/programming-languages-and-applied-logic/programming-haskell-2nd-edition)

### Equational reasoning

```haskell
add :: Nat -> Nat -> Nat
add Zero m = m
add (Suc n) m = Succ (add n m)

add Zero m == m (addZero)

add Zero (add y z) == add (add Zero y) z
== { applying addZero }
add y z = add (add Zero y) z
== { applying addZero }
add y z == add y z
== { reflexivity of (==) }
True
```
