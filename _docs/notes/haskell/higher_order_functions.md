---
layout: default
title: Higher-order functions
parent: Haskell
nav_order: 7
grand_parent: Notes
---

## Higher-order functions
Relevant content from Chapter 7 of [Programming in Haskell 2nd Edition](https://www.cambridge.org/us/academic/subjects/computer-science/programming-languages-and-applied-logic/programming-haskell-2nd-edition)

### Basic concepts

Functions with multiple arguments are usually defined using `currying`.
That is, arguments are taken one at a time by exploiting the fact that functions can return functions as results.

```haskell
add :: Int -> Int -> Int
add x y = x + y
-- means
add :: Int -> (Int -> Int)
add = \x -> (\y -> x + y)
```

It is also permissible to define a function that takes functions as arguments.

```haskell
twice :: (a -> a) -> a -> a
twice f x = f (f x)
```

```haskell
> twice (*2) 3
12

> twice reverse [1, 2, 3]
[1, 2, 3]
```

A function that takes a function as an argument is called a `higher-order function`.

### Processing lists

Standard prelude defines a number of `higher-order functions` for processing lists.

```haskell
map :: (a -> b) -> [a] -> [b]
map f xs = [f x | x <- xs]
```

```haskell
> map (+1) [1, 3, 5, 7]
[2, 4, 6, 8]

> map even [1, 2, 3, 4]
[False, True, False, True]

> map reverse ["abc", "def", "ghi"]
["ghi", "def", "abc"]
```

Map can be applied to itself to process nested lists.

```haskell
map (map (+ 1))
```


