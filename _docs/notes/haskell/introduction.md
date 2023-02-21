---
layout: default
title: Introduction
parent: Haskell
grand_parent: Notes
---

## Introduction

### Glasgow Haskell Compiler
[GHC](https://www.haskell.org/platform/) is the leading implementation of Haskell, and compromises both a compiler and interpreter.

### Starting GHCi

The interpreter can be started simply from the terminal command promt `$` by simply typing `ghci`

```haskell
$ ghci
GHCi, version 9.2.5: https://www.haskell.org/ghc/  :? for help
Prelude>
```

GHCi promt `>` means that the interpreter is now ready to evaluate an expression

It can now be used as a desktop calculator to evaluate simple numeric expressions

```haskell
> 2 + 3 * 4
14

> (2 + 3) * 4
20

> sqrt (3 ^ 2 + 4 ^ 2)
5.0
```

### The Standard Preulde

Haskell comes with a large number of standard library functions.

Including but not limited to numeric functions such as +, *, ^ as well as useful functions on lists.

```haskell
-- Returns the head (first element) of a list
> head [1, 2, 3, 4, 5]
1

-- Returns the tail of a list (everything but the first element)
> tail [1, 2, 3, 4, 5]
[2, 3, 4, 5]

-- Selects the nth element of a list
> [1, 2, 3, 4, 5] !! 2
3

-- Appending two lists
> [1, 2, 3] ++ [4, 5]
[1, 2, 3, 4, 5]

-- Calculate the length of a list
> length [1, 2, 3]
3

-- Reserve a list
> reverse [1, 2, 3]
[3, 2, 1]

-- Return the first n elements of a list
> take 3 [1, 2, 3, 4, 5]
[1, 2, 3]

-- Return the list without its first n elements
> drop 3 [1, 2, 3, 4, 5]

-- Calculate the sum of a list of numbers
> sum [1, 2, 3, 4, 5]
15

-- Calculate the product of a list of numbers
> product [1, 2, 3, 4, 5]
120
```

### Function Appliction Syntax

**TYPE**          | **MATHEMATICS**          | **HASKELL**                                                  |
-------------------- | -------------------- | -------------------- |
**FUNCTION APPLICATION** | Parantheses `f(a,b)`    | Space `f a b` |
**MULTIPLICATION** | Juxtaposition `cd` / Space `c d` | Multiplication symbol `c * d`  |

Moreover in Haskell function application is assumed to have higher priority than all other operators.

`f a + b` denotes `(f a) + b` not `f (a + b)`

**MATHEMATICS**          | **HASKELL**                                                  |
-------------------- | -------------------- |
`f(x)`    | `f x` |
`f(x,y)` | `f x y`|
`f(g(x))` | `f (g x)`|
`f(x,g(y))` | `f x (g y)`|
`f(x) g(y)` | `f x * g y`|

### Haskell Scripts

As well as functions in the standard library, it is also possible to define your own functions.

Theese are defined with a script (`.hs`), which is a text file comprosiing a sequence of definitions.

To start the created script GHCi can be used

```console
$ ghci test.hs
```

Now the standard file is loaded and its methods can be used. Once the file has been changed it has to be reloaded, because GHCi does not automatically detect that the script has been changed.

To do that the reload command must be executed before the newly added or changed definitions can be used.

**COMMAND**          | **MEANING**                                                  |
-------------------- | -------------------- |
`:load name`    | Loading script name |
`:reload` | Reloading current script |
`:set editor name` | Set editor to name |
`:edit name` | Set edit script name |
`:edit` | Edit current script |
`:type expr` | Show type of expr |
`:?` | Show all commands |
`:quit` | Quits GHCi |

### Naming Requirements

Function and argument names must begin with a lowercase letter.

```haskell
myFun, fun1, arg_2, x'
```

By convention, list arguments usually have an *s* suffix on their name.

```haskell
xs, ns, nss
```

### Layout Rule

In a sequence of definitions, each definition must begin in precisely the same column.

```haskell
a = 10
b = 20
c = 30
```

Whereas this would be invalid syntax.

```haskell
a = 10
    b = 10
c = 10
```
This avoids the need for explicit braces and semicolons.

Implicit grouping (Identation and white spaces relevant):

```haskell
a = b + c
    where
        b = 1
        c = 2
d = a * 2
```

Explicit grouing (With layout expanded, identation and white spaces not relevant)

```haskell
a = b + c
    where
    { b = 1;
      c = 2 }
d = a * 2
```
