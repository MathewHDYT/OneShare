---
layout: default
title: Interactive Programming
parent: Haskell
nav_order: 9
grand_parent: Notes
---

## Interactive programming
Relevant content from Chapter 10 of [Programming in Haskell 2nd Edition](https://www.cambridge.org/us/academic/subjects/computer-science/programming-languages-and-applied-logic/programming-haskell-2nd-edition)

### The problem

*Interactive programs* require input from the user. This is achieved while still staying in the realm of pure functions with a new type.

### The solution

An *interactive program* is viewed as a pure function that takes the current *state of the world* as its argument, and produces a modified world as its result. The modified world reflects any side effects that were performed by the program during execution.

Additionally, an *interactive program* may return a result in addition to performing side effects.

```haskell
type IO a = World -> (a, World)
```

Expressions of type `IO a` are called *actions*.

`IO ()` is the type of *actions* that returns the empty tuple `()` as a dummy result value
and can be though of as purely side-effecting actions that return no result value.

In addition to returning a result value, interactive programs may also require argument values.
This can be achieved using the notion of `currying`.

```haskell
type Char -> IO Int
-- Abbreviated from
type Char -> World -> (Int, World)
```

### Basic actions

Three basic `IO` actions that are predefined.

1. `getChar` reads a character from the keyboard, echoes it to the screen, and returns the character as its result value. If no character is waiting to be read, `getChar` waits until one is typed.

```haskell
getChar :: IO Char
getChar = ...
```

2. `putChar c` writes the character c to the screen ad returns no result value.

```haskell
putChar :: Char -> IO ()
putChar c = ...
```

3. `return v` returns the result value `v` without performing any interaction with the user. Provides a bridge between pure expressions without side effects to impure actions with side effects.

```haskell
return :: a -> IO a
return v = ...
```

Once a function is impure it cannot be made pure again, it already produced side effects.

### Sequencing

A sequence of `IO` actions can be combined into a single composite action using the `do` notation.

```haskell
do v1 <- a1
   v2 <- a2
   .
   .
   .
   vn <- an
   return (f v1 v2 ... vn)
```

First perform the action `a1` and call its result value `v1`. Then perform the action `a2` and call its result value `v2`, ...., then perform the action `an` and call its result value `vn`.

Finally, apply the function f to combine all the results into a single value which is then returned as the result value from the expression as a whole.

**Further, notes about the `do` notation:**
1. Each action in the sequence must begin in precisely the same column
2. The expressions `vi <- ai` are called `generators`, because they generate values for the variable `vi`
3. If the result value produced by a generator `vi <- ai` is not required, the generator can be abbreviated simply by `ai`, which is short form for `_ <- ai`

```haskell
act :: IO (Char, Char)
act = do x <- getChar -- Reads first character and saves it into x
         getChar -- Reads and then discards second character
         y <- getChar -- Reads third character and saves it into y
         return (x, y) -- Returns the results as a pair
```

Omitting `return` would result in a type error, because `(x, y)` is an expression of type `(Char, Char)`, whereas we require an action of type `IO (Char, Char)`.

### Derived primitives

Using the previously shown three basic actions with sequencing, there are additional useful `action primitives` in the standard prelude.

```haskell
-- Reads a string of characters from the keyboard, until terminated by the newline character '\n'
getLine :: IO String
getLine = do x <- getChar
             if x == '\n' then
               return []
             else
               do xs <- getLine
                  return (x:xs)

-- Write a string to the screen
putStr :: String -> IO ()
putStr [] = return ()
putStr (x:xs) = do putChar x
                   putStr xs

-- Write a string to the screen and also moves to a new line
putStrLn :: String -> IO ()
putStrLn xs = do putStr xs
                 putChar '\n'
```

These methods can now be used to create interactive programs.

```haskell
-- Prompts user for a string to be entered and displays its length
strlen :: IO ()
strlen = do putStr "Enter a string: "
            xs <- getLine
            putStr "The string has "
            putStr (show (length xs))
            putStrLn " characters"

> strlen
Enter a string: Haskell is a good programming language
The string has 38 characters
```
