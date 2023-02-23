---
layout: default
title: Types and classes
parent: Haskell
grand_parent: Notes
---

## Types and classes
Relevant content from Chapter 3 of [Programming in Haskell 2nd Edition](https://www.cambridge.org/us/academic/subjects/computer-science/programming-languages-and-applied-logic/programming-haskell-2nd-edition)

### Basic concepts
*Type* is a collection of related values. Usually displayed with the different notations explained below.


**NOTATION**         | **DESCRIPTION**      |
-------------------- | -------------------- |
`v :: T`             | `v` is a value in the type `T` |
`e :: T`             | `e` is an expression that has not yet been evaluated and will produce a value of type `T`|

Every expression needs a type, calculated prior to evaluating the expression.
This is done by a process called *type inference*.

`f` is a function that maps arguments of type `A` to arguments of type `B` and e is an expression of type `A`, then application `f e` has type `B`

```haskell
f :: A -> B, e :: A ==> f e :: B
```

*Type error* are deemed invalid expressions and happen when a nexxpression does not have a type.

```haskell
not :: Bool -> Bool, not 3 ==> not 3 :: Bool
```

This is invalid because the 3 is not of type `Bool`.

Theese types are checked before the evaluation, therefore Haskell programs are *type safe*.

### Basic types

**TYPE**             | **DESCRIPTION**      |  **EXAMPLES**      |
-------------------- | -------------------- | -------------------- |
`Bool`               | Logical values       | Contains `True` and `False` |
`Char`               | Single characters    | Contains all single characters in the Unicode system. Enclosed in single quotes `' '` |
`String`             | Strings of characters | Contains all sequences of characters and the empty string `""`. Enclosed in double quotes `" "` |
`Int`                | Fixed-precision integers | |
`Integer`            | Arbitrary-precision integers | |
