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
`Int`                | Fixed-precision integers | Contains integers, with a fixed amount of memory being used for storage. `GHC` range -2^63 - 2^63 - 1 |
`Integer`            | Arbitrary-precision integers | Contains integers with as much memory as necessary being used for storage |
`Float`            | Single-precision floating-point numbers | Contains numbers with decimal point, with fixed amount of memory being used for storage. Digits permitted after decimal point depends upon size of the number (7 total digits) |
`Integer`            | Double-precision floating-point numbers | Similair to `Float`, except twice as much memory is used to increase precision (14 total digits) |

### List types

Sequence of elements of the same type, while being enclosed in square parantheses and seperated by commas `[T, T, T]`.

Number of elements is called `length`. List `[]` of lengt zero is called empty list.

List of length one `[[]], [T]` are called singelton lists.

Type of the list only implies information about the type in the list. Not about its length because there is no requirement for it to be finite.

```haskell
[False, True, False] :: [Bool]
```

### Tuple types

Tuple is a finite sequence of components of possibly differnt types, while being enclosed in round parantheses and seperated by commas `(T1, T2)`.

Number of components in tuple is called `arity` Tuple `()` of `arity` zero is called empty tuple.

Tuples `(T1, T2)` of `arity` two is called pair. Tuples `(T1, T2, T3)` of `arity` three is called triples.

Tuples `(T1)` of `arity` one, however are not permitted, because they would conflict with the use of parantheses t make evaluation order explicit `(1 + 2) * 3`. 

Type of tuple `(Bool, Char)` conveys its `arity`. Furthermore there are no restrictions on types of components.

```haskell
(`a`, (False, `b`)) :: (Char, (Bool, Char))
([`a`, `b`], [False, True]) :: ([Char], [Bool])
[(`a`, False), (`b`, True)] :: [(Char, Bool)]
```

Tuples must have a finite `arity`, in order to ensure tuple types can be inferred priot to evaluation.

### Function types


