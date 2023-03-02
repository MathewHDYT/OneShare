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
('a', (False, 'b')) :: (Char, (Bool, Char))
(['a', 'b'], [False, True]) :: ([Char], [Bool])
[('a', False), ('b', True)] :: [(Char, Bool)]
```

Tuples must have a finite `arity`, in order to ensure tuple types can be inferred priot to evaluation.

### Function types

Function is a mapping `T1 -> T2` from `arguments` of one type to `results` of anyother type.

Simpe way to handle the case of multiple `arguments` and results, is by packaging multiple values using a `list` or `tuple`,
this is possible because there are no restrictions on the types of `arguments` and `results`.

There is no restriction that functions must be `total` on their argument type,
meaning there may be some arguments for which the result is not defined.

```haskell
> head []
*** Exception: Prelude.head: empty list
```

### Curried functions

Functions with multiple arguments can also be handled in another way.
More specifically by explotiting the fact that functions are free to return functions as results.

Consider the following definition:

```haskell
add :: (Int, Int) -> Int
add' :: Int -> (Int -> Int)
add' x y = x + y
```

The type state it is a function that takes an `argument` of type `Int` and returns a result that is a function of type `Int -> Int`.
More precisely it states that it takes an `Int` followed by another `Int` and returns the result of both `Int` combined.

The function `add` produces the same final result as `add'`, but `add` takes the two arguments packaged as a `pair` at once, `add'` takes them one at a time.

---------

Functions with more than just two arguments can be handled using the same way.

```haskell
-- 3 Arguments, 1 Result
mult :: Int -> (Int -> (Int -> Int))
mult x y z = x * y * z
```

These types of functions are called `curried functions`, the main advantage are that they are more flexible than their Tuple / List counterpart.
That is mainly because a `curried function` with only a partially filling the needed arguments and using that in combination with another function.

Furthemore excess parantheses can be dropped, because the function arrow `->` assumes association to the right.

```haskell
-- Explicit
:: Int -> (Int -> (Int -> Int))
-- Implicit
:: Int -> Int -> Int -> Int
```

Consequently function application, using spacing is assumed to associate to the left and unless tupling is explicitly required,
all functions with multiple arguments are normally defined as `curried functions`.

```haskell
-- Explicit
((mult x) y) z
-- Implicit
mult x y z
```

### Polymorphic types

To use a function on any element, irrespective of type, `polymorphic types` can be used.

```haskell
> length [1, 3, 5, 7]
4

> length ["Yes", "No"]
2

> length [sin, cos, tan]
3
```

This is done by the inclusion of a `type variable`. These must begin with a lower-case letter, usually simply named `a`, `b`, `c`, ... for example type of `length` is as follows.

```haskell
length :: [a] -> Int
```

For any type `a`, the function `length` has type `[a] -> Int`. A type that contians one or more type variables is called `polymorphic`.
Hence `[a] -> Int` is a `polymorphic` type / length is a `polymorphic` function.

### Overloaded types

To restrict `polymorphic types` to certain needed features and functionality `class constraint` can be used.
These are written in the form `C a`, where `C` is the anem of a class and `a` is a type variable.

```haskell
(+) :: Num a => a -> a -> a
```

In other words for any type `a` that is an `instance` of class `Num` the function `(+)` has type `a -> a -> a`.
A type / expression that contains one or more `class constraints` is called `overloaded.

### Basic classes

A class is a collection of types that support certain overloaded operations called methods.
All classes mentioned below are instanced by all basic types `Bool`, `Char`, `String`, `Int`, `Integer`, `Float` and `Double` as well as `list` and `tuple`.

#### Eq - equality types
Types whose values can be compared for equality and inequality.
Function types are **not** instances of the Eq class.

```haskell
(==) :: a -> a -> Bool
(/=) :: a -> a -> Bool
```

#### Ord - ordered types
Types that are instances of the Eq class and in addition whose values are totally (linearly) ordered.

```haskell
(<) :: a -> a -> Bool
(<=) :: a -> a -> Bool
(>) :: a -> -> Bool
(>=) :: a -> a -> Bool
min :: a -> a -> a
max :: a -> a -> a
```

#### Show - showable types
Types whose values can be converted into strings of characters.

```haskell
show :: a -> String
```

#### Read -readable types
Types whose values can be converted from strings of characters.

```haskell
read :: a -> String
```
