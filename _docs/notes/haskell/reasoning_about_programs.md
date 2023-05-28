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

reverse [x]
= { list notation }
reverse (x : [])
= { applying reverse }
reverse [] ++ [x]
= { applying reverse }
[] ++ [x]
= { pplying ++ }
[x]
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


