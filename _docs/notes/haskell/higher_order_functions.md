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
map (map (+ 1)) [[1, 2, 3], [4, 5]]
= { applying the outer map }
[map (+ 1) [1, 2, 3], map (+ 1) [4, 5]]
= { applying the inner maps }
[[2, 3, 4], [5, 6]]
```

The function `map` can also be defined using recursion.

```haskell
map :: (a -> b) -> [a] -> [b]
map f [] = []
map f (x:xs) = f x : map f xs
```

Another useful `higher-order` library funcion is `filter`, wich selects all elements of a list that satisfy a predicate, where a predicate (or property) is a funtion that returns a logical value.

```haskell
filter :: (a -> Bool) -> [a] -> [a]
filter p xs = [x | x <- xs, p x]
```

```haskell
> filter even [1..10]
[2, 4, 6, 8, 10]

> filter (> 5) [1..10]
[6, 7, 8, 9, 10]

-- /= means not equals
> filter (/= ' ') "abc def ghi"
"abcdefghi"
```

The function `filter` can be defined using recursion.

```haskell
filter :: (a -> Bool) -> [a] -> [a]
filter p [] = []
filter p (x:xs) | p x = x : filter p xs
                | otherwise = filter p xs
```

Other `higher-order functions` for processig lists that are defined in the standard prelude.

```haskell
-- Decides if all elements of a list satisfy a predicate
> all even [2, 4, 6, 8]
True

-- Decide if any element of a list satifies a predicate
> any odd [2, 4, 6, 8]
False

-- Select elements from a list while they satisfy a predicate
> takeWhile even [2, 4, 6, 7, 8]
[2, 4, 6]

-- 
> dropWhile odd [1, 3, 5, 6, 7]
[6, 7]
```

