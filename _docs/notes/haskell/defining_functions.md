---
layout: default
title: Defining functions
parent: Haskell
grand_parent: Notes
---

## Defining functions
Relevant content from Chapter 4 of [Programming in Haskell 2nd Edition](https://www.cambridge.org/us/academic/subjects/computer-science/programming-languages-and-applied-logic/programming-haskell-2nd-edition)

### New from old

Easiest way to define a new functon is to use already exisiting ones.

```haskell
even :: Integral a => a -> Bool
even n = n `div` 2 == 0

splitAt :: Int -> [a] -> ([a]. [a])
splitAt n xs = (take n xs, drop n xs)

recip :: Fractional a => a -> a
recip n = 1 / n
```

###  Conditional expressions

Haskell provides multiple ways to define functions, that choose between a number of possible results.
The simplest are `conditional expressions`, which use a logical expression called `condition` to choose between between two results of the same type.
If the `condition` is `True`, the first result is chosen, if it is `False` then the second result is chosen instead.

```haskell
abs :: Int -> Int
abs n = if n >= 0 then n else -n
```

`Conditional expressions` may be nested, meaning it can contain additional `conditional expressions`.

```haskell
signum :: Int -> Int
signum n = if n < 0 then -1 else
           if n == 0 then 0 else 1
```

`Conditional expression` must always have an `else`  branch, which avoids the `dangling else` problem.

### Guarded equations

Alternative to `conditional expressions`, in which a sequence of logical expressions called `guards` is used to choose between a sequence of results of the same types.

```haskell
abs n | n >= 0    = -1
      | n == 0    =  0
      | otherwise =  1
```

The symboy `|` os read as *such that* and the guard `otherwise` is defined in the standard prelude by `otherwise = True`.
Ending a sequence of quards with `otherwise` is not necessary, but provides a convenient way of handling all remaining cases,
as well as avoiding that none of the guards in the sequence is `True`, which results in an error.

Main benefit of `guarded equations` over `conditional expressions` is that multiple guards are easier to read.

```haskell
-- Conditional expressions
signum n = if n < 0 then -1 else
           if n == 0 then 0 else 1
-- Guarded equations
signum n | n < 0     = -1
         | n == 0    =  0
         | otherwise =  1
```

### Pattern matching

Sequence of syntactic expressions called `patterns` is used to choose between a sequence of results of the same type.

```haskell
not :: Bool -> Bool
not False = True
not True = False
```

Functions with multiple arguments can also be defined using `pattern matching`, in which case `patterns` for each argument are matched in order.

```haskell
(&&) :: Bool -> Bool -> Bool
True  && True  = True
True  && False = False
False && True  = False
False && False = False
```

The definition can be simplified by combining the last three equation into a single one always returning `False`,
independent of the values. This can be done using the wildcard pattern `_` that matches any value given.

```haskell
True && True = True
_    && _    = False
```

