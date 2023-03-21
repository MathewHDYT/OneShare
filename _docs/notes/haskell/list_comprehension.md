---
layout: default
title: List comprehension
parent: Haskell
nav_order: 5
grand_parent: Notes
---

## List comprehension
Relevant content from Chapter 5 of [Programming in Haskell 2nd Edition](https://www.cambridge.org/us/academic/subjects/computer-science/programming-languages-and-applied-logic/programming-haskell-2nd-edition)

### Basic concepts

The `list comprehension` notation can be used to construct new lists from existing lists.

```haskell
> [x^2 | x <- [1..5]]
[1, 4, 9, 16, 25]
```

The symbol `|` is read as *such that*, `<--` is read as *drawn from*, and the expression `x <- [1..5]` is called a *generator*.
A `list comprehension` can have more than one generator, with successive generators being separated by commas.

```haskell
> [(x, y) | x <- [1, 2, 3], y <- [4, 5]]
[(1, 4), (1, 5), (2, 4), (2, 5), (3, 4), (3, 5)]
```

Changing the order of the two generators in the previous example produces the same set of pairs, but arranged in a different order.

```haskell
> [(x, y) | y <- [4, 5], x <- [1, 2, 3]]
[(1, 4), (2, 4), (3, 4), (1, 5), (2, 5), (3, 5)]
```
This behavior can be understood by thinking of later generators as being more deeply nested, and hence changing the values of their variables more frequently than earlier generators.

Later generators can also depend upon the values of variables from an earlier generator.

```haskell
> [(x, y) | x <- [1..3], y <- [x..3]]
[(1, 1), (1, 2), (1, 3), (2, 2), (2, 3), (3, 3)]

concat :: [[a]] -> [a]
concat xss = [x | xs <- xss, x <-- xs]
```

The `wildcard pattern` can be used in generators to discard certain elements from a list.

```haskell
firsts :: [(a, b)] -> [a]
firsts ps = [x | (x, _) <- ps]
length :: [a] -> Int
length xs = sum [1 | _ <- xs]
```

### Guards

`List comprehensions` can also use logical expressions called `guards` to filter values produced by previous generators.
If a guard is `True`, then the current values will be retained, if it is `False` they are discarded instead.

```haskell
evens :: Int -> [Int]
evens n = [x | x <- [1..n], even x]

> evens 10
[2, 4, 6, 8, 10]

factors :: Int -> [Int]
factors n = [x | x <- [1..n], n 'mod' x == 0]

> factors 15
[1, 3, 5, 15]

> factors 7
[1, 7]

-- Does not require the function factors to produce all of its factors, because under lazy evaluation
-- the result False is returned as soon as any factor other than one or the number itself is produced.
prime :: Int -> Bool
prime n = factors n == [1, n]

> prime 15
False

> prime 7
True

primes :: Int -> [Int]
primes n = [x | x <- [2..n], prime x]

> primes 40
[2, 3, 5, 7, 11, 13, 17, 19, 23, 29, 31, 37]

find :: Eq a => a -> [(a, b)] -> [b]
find k t = [v | (k', v) <- t, k == k']

> find 'b' [('a', 1), ('b', 2), ('c', 3), ('b', 4)]
[2, 4]
```

### The `zip` function

Produces a new list by pairing successive elements from two existing lists until either or both lists are exhausted.

```haskell
> zip ['a', 'b', 'c'] [1, 2, 3, 4]
[('a', 1), ('b', 2), ('c', 3)]
```

Useful when programming with `list comprehensions`.

```haskell
pairs :: [a] -> [(a, a)]
pairs xs = zip xs (tail xs)

> pairs [1, 2, 3, 4]
[(1, 2), (2, 3), (3, 4)]

-- Does not require the function pairs to produce all pairs, because under lazy evaluation
-- the result False is returned as soon as any non-ordered pair os produced.
sorted :: Ord a => [a] -> Bool
sorted xs = and [x <= y | (x, y) <- pairs xs]

> sorted [1, 2, 3, 4]
True

> sorted [1, 3, 2, 4]
False

positions :: Eq a => a -> [a] -> [Int]
positions x x = [i | (x', i) <- zip xs [0..], x == x']

> positions False [True, False, True, False]
[1, 3]
```

`[0..]` produces list of indices `[0, 1, 2, 3, ...]` being notionally `infinite`, but under lazy evaluation only as many elements of the list as required by the context will be produced.
Using lazy evaluation in this manner avoids the need to be explicitly produce a list of indices of the same length as the input list.

### String comprehensions

Strings are not primitive, but are constructed as lists of characters. The string `"abc" :: String` is just an abbreviation for the list of characters `['a', 'b', 'c'] :: [Char]`.
Because strings are lists, any polymorphic function on lists can also be used with strings.


```haskell
> "abcde" !! 2
'c'

> take 3 "abcde"
"abc"

> length "abcde"
5

> zip "abc" [1, 2, 3, 4]
[('a', 1), ('b', 2), ('c', 3)]
```

For the same reason, `list comprehensions` can also be used to define functions on strings.

```haskell
lowers :: String -> Int
lowers xs = length [x | x <- xs, x >= 'a' && x <= 'z']

> lowers "Haskell"
6

count :: Char -> String -> Int
count x xs = length [x' | x' <- xs, x == x']

> count 's' "Mississippi"
4
```
