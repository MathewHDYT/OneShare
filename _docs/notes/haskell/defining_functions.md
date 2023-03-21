---
layout: default
title: Defining functions
parent: Haskell
nav_order: 4
grand_parent: Notes
---

## Defining functions
Relevant content from Chapter 4 of [Programming in Haskell 2nd Edition](https://www.cambridge.org/us/academic/subjects/computer-science/programming-languages-and-applied-logic/programming-haskell-2nd-edition)

### New from old

Easiest way to define a new function is to use already existing ones.

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
The simplest are `conditional expressions`, which use a logical expression called `condition` to choose between two results of the same type.
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

The symbol `|` is read as *such that* and the guard `otherwise` is defined in the standard prelude by `otherwise = True`.
Ending a sequence of guards with `otherwise` is not necessary, but provides a convenient way of handling all remaining cases,
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

This version has the benefit that under `lazy evaluation`, if the first argument is `False`, then the second one does not need to be evaluated.

```haskell
True  && True = True
False && _    = False
```

Prelude defines `&&` like this, that is if the first argument is `True`, then the result is the value of the second argument. If the argument is `False` then the result is `False`.

```haskell
True  && b = b
False && _ = False
```

Note that using the same name to be used for more than one argument in a single equation is not permitted.

### Tuple patterns

Is a pattern, that matches any tuple of the same `arity`,
whose component all match the corresponding patterns in order.

The library functions `fst` and `snd`, they respectively select the first and second components of a pair.

```haskell
fst :: (a, b) -> a
fst (x, _) = x

snd :: (a, b) -> b
snd (_, y) = y
```

### List patterns

Is a pattern, that matches any list of the same `length`,
whose elements all match the corresponding patterns in order.

The function below called `test` decides if a list contains precisely three characters beginning with the letter `a`.

```haskell
test :: [Char] -> Bool
test [`a`, _, _] = True
test _           = False
```

Lists are not primitive as such, but are instead constructed one element at a time.

Starting from the empty list `[]` using an operator `:` called *cons* that *con*structs a new list by prepending a new element to the start of an existing list.

```haskell
[1, 2, 3]
= {list notation}
1 : [2, 3]
= {list notation}
1 : (2 : [3])
= {list notation}
1 : (2 : (3 : []))
```

Meaning `[1, 2, 3]` is an abbreviation for `1 : (2 : (3 : []))`. The *cons* operator is assumed to operate to the right.
`1 : 2 : 3 : []` means `1 : (2 : (3 : []))`.

The *cons* operator can be used to construct patterns, which match any non-empty list whose first and remaining elements match the corresponding patterns in order.

```haskell
test :: [Char] -> Bool
test (`a`:_) = True
test _         = False

head :: [a] -> a
head (x:_) = x

tail :: [a] -> [a]
tail (_:xs) = xs
```

*Cons* patterns must be parenthesized, because function application has a higher priority than all other operators.

### Lambda expression

Allows constructing functions, as an alternative to defining functions.
Compromises a pattern for each of the arguments, a body that specifies how the result is calculated, but does not give a name to the constructed function itself.

```haskell
\x -> x + x
```

The symbol `\` represents the Greek letter `lambda`.

Despite the fact that `lambda expressions` do not have names, they can still be used in the same way as any other function

```haskell
> (\x -> x + x) 2
4
```

Firstly, they can be used to formalize the meaning of `curried function` definitions.

```haskell
-- Curried function
add :: Int -> Int -> Int
add x y = x + y
-- Lambda expression
add' :: Int -> (Int -> Int)
add = \x -> (\y -> x + y)
```

Makes precise that `add` is a function that takes an integer `x` and returns a function,
that function in turn takes another integer `y` and returns the result `x + y`.

Secondly, they are useful when defining functions that return functions as results by their very nature, rather than as a consequence of currying.

```haskell
-- Curried function
const :: a -> b -> a
const x _ = x
-- Lambda expression
const :: a -> (b -> a)
const x = \_ -> x
```

Finally, they can be used to avoid having to name a function that is only referenced once in a program.

```haskell
odds :: Int -> [Int]
odds n = map f [0..n-1]
         where f x = x * 2 + 1
```

Locally defined function `f` is only referenced once, therefore the definition can be simplified.

```haskell
odds :: Int -> [Int]
odds n = map (\x -> x * 2 + 1) [0..n-1]
```

### Operator sections

Functions that are written between their two arguments (`+`, `*`, ...) are called `operators`.
Any function with two arguments can be converted into a `operator` by enclosing the name of the function in a single back quote `\``.

The opposite is possible as well any `operator` can be converted into a `curried function` by enclosing the name of the `operator` in parentheses `()`.
Moreover, this also allows one of the arguments to be included in the parentheses if desired (`(1 +) 2`, `(+ 2) 1`).
In general this also allows the expressions of the form `(#)`, `(x #)`, and `(# y)`, for arguments `x` and `y` are called `sections`.

```haskell
(#) = \x -> (\y -> x # y)
(x #) = \y -> x # y
(# y) = \x -> x # y
```

Sections can be used to construct a number of simple but useful functions, in a particular compact way.

```haskell
(+) = \x -> (\y -> x + y)
(1+) = \y -> 1 + y
(1/) = \y -> 1 / y
(*2) = \x -> x * 2
(/2) = \x -> x / 2
```

Furthermore, they are necessary when stating the type of operator, because an operator itself is not a valid expression.

```haskell
(+) :: Int -> Int -> Int
```

Finally, they are necessary when using operators as arguments to other functions.

```haskell
sum :: [Int] -> Int
sum = foldl (+) 0
```
