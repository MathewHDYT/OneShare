---
layout: default
title: Reasoning about programs
parent: Haskell
nav_order: 11
grand_parent: Notes
---

## Reasoning about programs
Relevant content from Chapter 16 of [Programming in Haskell 2nd Edition](https://www.cambridge.org/us/academic/subjects/computer-science/programming-languages-and-applied-logic/programming-haskell-2nd-edition)

### Equational reasoning

Basic algebraic properties of numbers can have a computational significance.
For example, the expression `x (y + z)` requires two operations, whereas the equivalent expression `x y + x z` requires three operations.
Hence, the first term is more efficient.

```haskell
x y = y x
x + (y + z) = (x + y) + z
x (y + z) = x y + x z
(x + y) z = x z + y z
```

### Reasoning about Haskell

In order to simplify the process of reasoning about programs it is good practice to use `non-overlapping` patterns, which refer to patterns that do not rely on the order in which they are matched, whenever possible when defining functions.

```haskell
-- Overlapping
isZero :: Int -> Bool
isZero 0 = True
isZero n = False

-- Non-overlapping
isZero' :: Int -> Bool
isZero' 0 = True
isZero' n | n /= 0 = False
```

### Simple examples

Using this definition, we can show that `reverse` has no effect on singleton list, in the sense that any expression of the form `reverse [x]` in a program can be freely replaced by `[x]` without change in meaning,
but with a change in efficiency by avoiding need to apply the reverse function.

```haskell
reverse :: [a] -> [a]
reverse [] = []
reverse (x:xs) = reverse xs ++ [x]

reverse [x] == [x]
= { list notation }
reverse (x : []) == [x]
= { applying reverse }
reverse [] ++ [x] == [x]
= { applying reverse }
[] ++ [x] == [x]
= { applying ++ }
[x] == [x]
== { reflexivity of (==) }
True
```

### Induction on numbers

Involves some form of recursion. Reasoning about such programs using `induction`.

```haskell
data Nat = Zero | Succ Nat
```

To prove some property `p`, holds for all natural numbers, the principle of induction states that it is sufficient to show that `p` holds for `Zero`,
called `base case`, and that `p` is preserved by `Succ`, called `inductive case`.

```haskell
add :: Nat -> Nat -> Nat
add Zero m = m
add (Suc n) m = Succ (add n m)
```

Induction on `n` to prove that dual property, `add n Zero = n` holds for all natural numbers `n`.

```haskell
-- Base case (Zero)
add Zero m == m
== { applying addZero }
m == m
== { reflexivity of (==) }
True

-- Inductive case (Succ n)
add (Succ n) Zero == Succ n
== { applying addSucc }
Succ (add n Zero) == Succ n
== { induction hypothesis }
Succ n == Succ n
== { reflexivity of (==) }
= True
```

Induction on `x` to proof associativity of `add`, `add x (add y z) = add (add x y) z` holds for all `x`, `y` and `z`.

```haskell
-- Base case (Zero)
add Zero (add y z) == add (add Zero y) z
== { applying addZero }
add y z = add (add Zero y) z
== { applying addZero }
add y z == add y z
== { reflexivity of (==) }
True

-- Inductive case (Succ x)
add (Succ x) (add y z) == add (add (Succ x) y) z
== { applying addSucc }
Succ (add x (add y z)) = add (add (Succ x) y) z
== { induction hypothesis }
Succ (add (add x y) z) = add (add (Succ x) y) z
== { unapplying addSucc }
add (Succ (add x y)) z = add (add (Succ x) y) z
== { unapplying addSucc }
add (add (Succ x) y) z = add (add (Succ x) y) z
== { reflexivity of (==) }
True
```

### Introduction on lists

Induction is not restricted to natural numbers, but can also be used to reason about other recursive types.

To prove some property `p`, holds for all lists, the principle of induction states that it is sufficient to show that `p` holds for `[]`,
called `base case`, and that `p` it holds for any list `xs`, then it also holds for `x:xs` for any element x, called `inductive case`.

Induction on `xs` to prove that dual property, `reverse (reverse xs) = xs` holds for all lists `xs`.

```haskell
-- Base case ([])
reverse (reverse []) == []
== { applying reverse [] }
reverse [] == []
== { applying reverse [] }
[] == []
== { reflexivity of (==) }
True

-- Inductive case (x:xs)
reverse (reverse (x:xs)) == x : xs
== { applying reverse (x:xs) }
reverse (reverse xs ++ [x]) == x : xs
== { distributity of reverse }
reverse [x] ++ reverse (reverse xs) == x : xs
== { applying reverse [x] (singleton list) }
[x] ++ reverse (reverse xs) == x : xs
== { induction hypothesis }
[x] ++ xs == x : xs
== { applying ++ }
x : xs == x : xs
== { reflexivity of (==) }
True
```

Uses two auxiliary properties of the function `reverse`.

1. `reserve` preserves singleton lists: `reverse [x] = [x]`
2. `reserve` distributes over append, expect that order of the two argument lists is swapped (`contravariant`): `reverse (xs ++ ys) = reverse ys ++ reverse xs`

```haskell
-- Base case ([])
reverse ([] ++ ys) == reverse ys ++ reverse []
== { applying ++ }
reverse ys == reverse ys ++ reverse []
== { identity for ++ }
reverse ys ++ [] == reverse ys ++ reverse []
== { unapplying reverse [] }
reverse ys ++ reverse [] == reverse ys ++ reverse []
== { reflexivity of (==) }
True

-- Inductive case (x:xs)
reverse ((x:xs) ++ ys) == reverse ys ++ reverse (x:xs)
== { applying ++ }
reverse (x : (xs ++ ys)) == reverse ys ++ reverse (x:xs)
== { applying reverse }
reverse (xs ++ ys) ++ [x] == reverse ys ++ reverse (x:xs)
== { induction hypothesis }
(reverse ys ++ reverse xs) ++ [x] == reverse ys ++ reverse (x:xs)
== { associativity of ++ }
reverse ys ++ (reverse xs ++ [x]) == reverse ys ++ reverse (x:xs)
== { unapplying reverse xs ++ [x] }
reverse ys ++ reverse (x:xs) == reverse ys ++ reverse (x:xs)
== { reflexivity of (==) }
True
```

### Making append vanish

Many recursive functions are defined using the `append` operator `++` on lists. This operator carries a considerable efficiency cost when used recursively tough.

```haskell
reverse :: [a] -> [a]
reverse [] = []
reverse (x:xs) = reverse xs ++ [x]
```

The efficiency of is this version of `reverse` is quadratic time in the length of its argument.
To increase the efficiency we attempt to define a more general function, which combines the behaviors of `reverse` and `++`.

```haskell
reverse' xs ys = reverse xs ++ ys

-- Base case ([])
reverse' [] ys = ys
== { specification of reverse' }
reverse [] ++ ys == ys
== { applying reverse }
[] ++ ys == ys
== { applying ++ }
ys == ys
== { reflexivity of (==) }
True

-- Inductive case (x:xs)
reverse' (x:xs) ys == reverse' xs (x : ys)
== { specification of reverse' }
reverse (x:xs) ++ ys == reverse' xs (x : ys)
== { applying reverse }
(reverse xs ++ [x]) ++ ys == reverse' xs (x : ys)
== { associativity of ++ }
reverse xs ++ ([x] ++ ys) == reverse' xs (x : ys)
== { induction hypothesis }
reverse' xs ([x] ++ ys) == reverse' xs (x : ys)
== { applying ++ }
reverse' xs (x : ys) == reverse' xs (x : ys)
== { reflexivity of (==) }
True

reverse' :: [a] -> [a] -> [a]
reverse' [] ys = ys
reverse' (x:xs) ys = reverse' xs (x : ys)

reverse :: [a] -> [a]
reverse xs = reverse' xs []

reverse [1, 2, 3]
= { applying reverse }
reverse' [1, 2, 3] []
= { applying reverse' }
reverse' [2, 3] (1: [])
= { applying reverse' }
reverse' [3] (2 : (1 : []))
= { applying reverse' }
reverse' [] (3 : (2 : (1: [])))
= { applying reverse' }
3 : (2 : (1: []))
= { applying list notation }
[3, 2, 1]
```

The list is reversed by using an extra argument to accumulate the final result. This version might be less clear but much more efficient.
The number of reduction steps required to evaluate `reverse xs` for a list of length `n` is only `n + 2`. Taking only linear time in the length of its argument instead of quadratic like before.
