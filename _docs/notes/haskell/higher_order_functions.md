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

A function that takes a function as an argument is called a `higher-order` function.

### Processing lists

Standard prelude defines a number of `higher-order` functions for processing lists.

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

Another useful `higher-order` library function is `filter`, which selects all elements of a list that satisfy a predicate, where a predicate (or property) is a function that returns a logical value.

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

Other `higher-order` functions for processing lists that are defined in the standard prelude.

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

-- Remove elements from a list while they satisfy a predicate
> dropWhile odd [1, 3, 5, 6, 7]
[6, 7]
```

### The `foldr` function

Many functions that take a list as their argument can be defined using recursion.

```haskell
f [] = v
f (x:xs) = x # f xs
```

The function maps the empty list to a value `v` and any non-empty list to an operator `#` applied to the head of the list
and the result of recursively processing the tail.

```haskell
sum [] = 0
sum (x:xs) = x + sum xs

product [] = 1
product (x:xs) = x * product xs

or [] = False
or (x:xs) = x || or xs

and [] = True
and (x:xs) = x && and xs
```

The `higher-order` library function `foldr` (*fold right*) encapsulated this pattern.
With the operator `#` and the value `v` as arguments.

```haskell
sum :: Num a => [a] -> a
sum = foldr (+) 0

product :: Num a => [a] -> a
product = foldr (*) 1

or :: [Bool] -> Bool
or = foldr (||) False

and :: [Bool] -> Bool
and = foldr (&&) True
```

The new definition can also include explicit list arguments

```haskell
sum xs = foldr (+) 0 xs
```

The `foldr` function itself can be defined using recursion.

```haskell
foldr :: (a -> b -> b) -> b -> [a] -> b
foldr f v [] = v
foldr f v (x:xs) = f x (foldr f v xs)
```

It is best to think of `foldr f v` in a non-recursive manner,
as simply replacing each cons operator in a list by the function f, and the empty list at the end by the value v.

```haskell
-- foldr (+) 0
1 : (2 : (3 : []))
1 + (2 + (3 + 0))
```

`foldr` can be used to define many more functions and reflects an operator that is assumed to associate to the right.

```haskell
length :: [a] -> Int
length = foldr (\_ n -> 1 + n) 0
```

`fold right` 

### The `foldl` function

It is possible to define recursive functions on lists using an operator that is assumed to associate to the left.

```haskell
sum :: Num a => [a] -> a
sum = sum' 0
      where
        sum' v [] = v
        sum' v (x:xs) = sum' (v + x) xs
```

```haskell
sum [1, 2, 3]
= { applying sum }
sum' 0 [1, 2, 3]
= { applying sum' }
sum' (0 + 1) [2, 3]
= { applying sum' }
sum' ((0 + 1) + 2) [3]
= { applying sum' }
sum' (((0 + 1) + 2) + 3) []
= { applying sum' }
(((0 + 1) + 2) + 3) 
= { applying + }
6
```

The order of association does not affect the result in this case, because addition is associative.
That is `x + (y + z) = (x + y) + z` for any number `x`, `y` and `z`.

Many functions on list can be defined using the following simple pattern of recursion.

```haskell
f v [] = v
f v (x:xs) = f (v # x) xs
```

The function maps the empty list to the `accumulator` value `v` and any non-empty list to the result of recursively processing the tail,
using a new `accumulator` value obtained by applying an operator `#` to the current value and the head of the list.

The `higher-order` library function `foldl` (*fold left*) encapsulated this pattern.
With the operator `#` and the `accumulator` `v` as arguments.

```haskell
sum :: Num a => [a] -> a
sum = foldl (+) 0

product :: Num a => [a] -> a
product = foldl (*) 1

or :: [Bool] -> Bool
or = foldl (||) False

and :: [Bool] -> Bool
and = foldl (&&) True
```

The additional function defined above can also be rewritten to use `foldl` instead of `foldr`.

```haskell
length :: [a] -> Int
length = foldl (\n _ -> n + 1) 0

reverse :: [a] -> [a]
reverse = foldl (\xs x -> x:xs) []
```

```haskell
length [1, 2, 3] = ((0 + 1) + 1) + 1 = 3
reverse [1, 2, 3] = 3 : (2 : (1 : [])) = [3, 2, 1]
```

When a function can be defined using both `foldr` and `foldl`, the choice of which definition is preferable is made on grounds of efficiency
and therefore requires consideration of the evaluation mechanism.

The `foldl` function itself can be defined using recursion.

```haskell
foldl :: (a -> b -> a) -> a -> [b] -> a
foldl f v [] = v
foldl f v (x:xs) = foldl f (f v x) xs
```

It is best to think of `foldl f v` in a non-recurisve manner.
In terms of an operator `#` that is assumed to associate to the left.

### The composition operator

The `higher-order` library operator `.` returns the composition of two functions as a single function.

```haskell
(.) :: (b -> c) -> (a -> b) -> (a -> c)
f . g = \x -> f (g x)
```

That is `f . g` read as `f` *composed with* `g`, is the function that takes an argument `x`,
applies the function `g` to it and applies the function `f` to the result.

Composition can be used to simplify nested function applications by reducing parentheses and avoiding the need to explicitly refer to the initial argument.

```haskell
-- Non composite version
odd n = not (even n)
twice f x = f (f x)
sumsqreven ns = sum (map (^ 2) (filter even ns))

-- Version making use of composition
odd = not . even
twice f = f . f
sumsqreven = sum . map (^ 2) . filter even
```

The last definition exploits the fact that composition is associative, meaning `f . (g . h) = (f . g) . h` for any function `f`, `g` and `h`.

Composition also has an identity.

```haskell
id :: a -> a
id = \x -> x
```

That is `id` is the function that simply returns its argument unchanged and has the property `id . f = f` and `f . id = f` for any function `f`.
Useful as a suitable starting point for a sequence of compositions.

```haskell
compose :: [a -> a] -> (a -> a)
compose = foldr (.) id
```
