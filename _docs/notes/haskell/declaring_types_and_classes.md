---
layout: default
title: Declaring types and classes
parent: Haskell
nav_order: 8
grand_parent: Notes
---

## Declaring types and classes
Relevant content from Chapter 8 of [Programming in Haskell 2nd Edition](https://www.cambridge.org/us/academic/subjects/computer-science/programming-languages-and-applied-logic/programming-haskell-2nd-edition)

### Type declaration

The simplest way to delare a new type is to introduce a new name for an existing type.

This can be done using the `type` mechanism.

```haskell
type String = [Char]
```

The name of the new type must begin with a capital letter.

Type declarations can be nested, meaning one type can be declared in terms of another.

```haskell
type Pos = (Int, Int)
type Trans = Pos -> Pos
```

Type declarations cannot be recursive. Recursive types can instead be declread using the `data` mechanism.

```haskell
type Tree = (Int, [Tree])
```

Type declarations can be parameterised by other types. Where the same type has to be used on multiple different places.

```haskell
type Pair a = (a, a)
```

Type declarations with more than one parameter are possible as well.

```haskell
type Assoc k v = [(k, v)]

find :: Eq k => k -> Assoc k v -> v
find k t = head [v | (k', v) <- t, k == k']
```

### Data declarations

A complete new type, can be declared by specifying its value using the `data` mechanism.

```haskell
data Bool = False | True
```

The symbol `|` is read as `or`, and the new values of the type are called `constructors`.

As with new types, the names of `constructors` must begin with capital letters and the same construcotrs cannot be used in more than one type.

Values of new types can be used on the same way as those of built-in types.
- Freely passed as arguments to functions
- Returned as results from functions
- Stored in data structures
- Used in patterns

```haskell
data Move = North | South | East | West

move :: Move -> Pos --> Post
move North (x, y) = (x, y + 1)
move South (x, y) = (x, y - 1)
move East  (x, y) = (x + 1, y)
move West  (x, y) = (x - 1, y)

moves :: [Move] -> Pos -> Pos
moves [] p = p
moves (m:ms) p = moves ms (move m p)

rev :: Move -> Move
rev North = South
rev South = North
rev East = West
rev West = East
```

The `constructors` in a data delcaration can also have arguments.

In that case they become `constructor functions` using `currying`.

```haskell
data Shape = Circle Float | Rect Float Float
-- Shape values of form:
-- Circle r r (r = floating-point number)
-- Rect x y (x, y = floating-point number)

square :: Float -> Shape
square n = Rect n n

area :: Shape -> Float
area (Circle r) = pi * r ^ 2
area (Rect x y) = x * y

> :type Circle
Circle :: Float -> Shape

> :type Rect
Rect :: Float -> Float -> Shape
```

The different between normal functions and `constructor functions`, is that the latter exists purely for building pieces of data.

```haskell
-- Can be evaluated with defining equation of negate
negate 1.0
-1.0

-- Can not be evaluated no defining equation of Circle exists
Circle 1.0
```

Declarations can also be parameterised. 

```haskell
data Maybe a = Nothing | Just a
-- Maybe values of form:
-- Nothing
-- Just x (x = some value x of type a)
```

`Maybe a` can be though of as values that either fail or succeed.

```haskell
safediv :: Int -> Int -> Maybe Int
safediv _ 0 = Nothing
safediv m n = Just (m `div` n)

safehead :: [a] -> Maybe a
safehead [] = Nothing
safehead xs = Just (head xs)
```

### Newtype declarations

If a new type has a single constructor with a single argument, it can be declared using the `newtype` mechanism.

```haskell
newtype Nat = N Int
type Nat = Int
data Nat = N Int
```

Using `newtype` rather than `type` means that `Nat` and `Int` are different types rather than synonyms.

Meaning they cannot accidentally be mixed up and `newtype` rather than `data` is more efficient, because `newtype` constructors such as `N` do no incur nay cost when the program is evaluated.

This is possible because the y are removed by the compiler once type checking is completed.

### Recursive types

New types declared using the `data` and `newtype` mechanisms can also be reursive.

```haskell
data Nat = Zero | Succ Nat
-- Nat values of form:
-- Zero / 0
-- Succ n (n = sone value n of type Nat) / (1+)

Zero
Succ Zero
Succ (Succ Zero)
Succ (Succ (Succ Zero))
...

nat2int :: Nat -> Int
nat2int Zero = 0
nat2int (Succ n) = 1 + nat2int n

int2nat :: Int -> Nat
int2nat 0 = Zero
int2nat n = Succ (int2nat (n - 1))

add :: Nat -> Nat -> Nat
add m n = int2nat (nat2int m + nat2int n)
```


