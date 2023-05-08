---
layout: default
title: Monads and more
parent: Haskell
nav_order: 10
grand_parent: Notes
---

## Monads and more
Relevant content from Chapter 12 of [Programming in Haskell 2nd Edition](https://www.cambridge.org/us/academic/subjects/computer-science/programming-languages-and-applied-logic/programming-haskell-2nd-edition)

### Functors

Are examples of abstracting out a common programming pattern as a definition.

```haskell
inc :: [Int] -> [Int]
inc [] = []
inc (n:ns) = n + 1 : inc ns

sqr :: [Int] -> [Int]
sqr [] = []
sqr (n:ns) = n ^ 2 : sqr ns
```

Both functions are defined in the same manner, with empty list being mapped to itself in the base case,
and a non-empty list to some function applied to the head of the list and the result of recursively processing the tail.

Abstracting out this pattern gives the library function `map`.

```haskell
map :: (a -> b) -> [a] -> [b]
map f [] = []
map f (x:xs) = f x : map f xs
```

This function can now be used to create our two examples more compactly.

```haskell
inc = map (+ 1)
sqr = map (^ 2)
```

More generally the idea of mapping functions over each element of a data structure isn't specific to the type of lists, but can be abstracted further to a wide range of parameterized types.
The class of types that support such a mapping function are called `functors`.

```haskell
class Functor f where
    fmap :: (a -> b) -> f a -> f b
```

For a parameterized type `f` to be an instance of the class `Functor`, it must support a function `fmap` of the given type.
Meaning `fmap` takes a function of type `a -> b` and a structure of type `f a`, whose elements have type `a` sometimes called `container type,` and applies the function to each element to give a structure of type `f b` whose elements now have type `b`.

```haskell
instance Functor [] where
    -- fmap :: (a -> b) -> f a -> f b
    fmap = map
```

`[]` denotes the list type without a type parameter in this declaration, and is based upon the fact that the type `[a]` can also be written as the application of `[] a` of the list type `[]` to the parameter type `a`.

```haskell
data Maybe a = Nothing | Just a

instance Functor Maybe where
    -- fmap :: (a -> b) -> f a -> f b
    fmap _ Nothing = Nothing
    fmap g (Just x) = Just (g x)

> fmap (+ 1) Nothing
Nothing

> fmap (* 2) (Just 3)
Just 6

> fmap not (Just False)
Just True
```

User-defined types can also be made into functors.

```haskell
data Tree a = Leaf a | Node (Tree a) (Tree a)
              deriving Show

instance Functor Tree where
    -- fmap :: (a -> b) -> f a -> f b
    fmap g (Leaf x) = Leaf (g x)
    fmap g (Node l r) = Node (fmap g l) (fmap g r)

> fmap length (Leaf "abc")
Leaf 3

> fmap even (Node (Leaf 1) (Leaf 2))
Node (Leaf False) (Leaf True)
```

As well as the `IO` type.

```haskell
instance Functor IO where
    -- fmap :: (a -> b) -> f a -> f b
    fmap g mx = do x <- mx
                   return (g x)

> fmap show (return False)
"True"
```

**The two key benefits of functors:**

1. Function `fmap` can be used to process the elements of any structure that is `functorial`
2. Possibility of defining generic functions that can be used with any functor

```haskell
inc :: Functor f => f Int -> f Int
inc = fmap (+ 1)

> inc (Just 1)
Just 2

> inc [1, 2, 3, 4, 5]
[2, 3, 4, 5, 6]

> inc (Node (Leaf 1) (Leaf 2))
Node (Leaf 2) (Leaf 3)
```

### Functor laws

In addition to providing the function `fmap`, functors are also required to satisfy two equational laws.

```haskell
-- (f a -> f a)
fmap id = id
-- (b -> c) -> (a -> b)
fmap (g . h) = fmap g . fmap h
```

1. `fmap` preserves the identity function, applying `fmap` to `id` returns the same as the function `id` by itself
2. `fmap` preserves function composition, applying `fmap` to the composition of two functions gives the same result as applying `fmap` to the functions separately and then composing

In combination with the polymorphic type for `fmap`, the functor laws ensure that `fmap` does indeed perform a mapping operation.

In the case of lists they ensure that the structure of the argument list is preserved by `fmap`, meaning no element is added, removed or rearranged.

```haskell
instance Functor [] where
    -- fmap :: (a -> b) -> f a -> f b
    fmap g [] = []
    fmap g (x:xs) = fmap g xs ++ [g x]

> fmap id [1, 2]
[2, 1]

> id [1, 2]
[1, 2]

> fmap (not . even) [1, 2]
[False, True]

> (fmap not . fmap even) [1, 2]
[True, False]
```

There is at most one function `fmap` that satisfies the required laws. If it is possible to make a given parameterized type into a `functor`, there is only one way to achieve it.

### Applicatives

Are examples of generalize the idea of `Functors` to allow any functions with any number of arguments to be mapped, rather than being restricted to functions with a single argument.
We can use the notion of currying to achieve this.

```haskell
-- Converts a value of type a into a structore of type f a
pure :: a -> f a
-- Generalised form of function application, where argument function / value and the result valie are allc ontained in f structures
(<*>) :: f (a -> b) -> f a -> f b
-- Assumed to associate to the left
g <*> x <*> y <*> z
((g <*> x) <*> y) <*> z
```

A typical use of `pure` and `<*>` has the following form.

```haskell
pure g <*> x1 <*> x2 <*> ... <*> xn
```

Such expressions are in `applicative style`, because of the similarity to normal function application notation `g x1 x2 ... xn`.
Where in both cases `g` is a curried function that takes `n` arguments of type `a1 ... an` and produces a result of type `b`.

In `applicative style`, each argument `xi` has type `f ai` rather than just `ai` and the overall result has type `f b` rather than `b`.
Using this idea we can define the hierarchy of mapping functions.

```haskell
-- Degenerate case when function being mapped has no argument
fmap0 :: a -> f a
fmap0 = pure

-- Another name for fmap
fmap1 :: (a -> b) -> f a -> f b
fmap1 g x = pure g <*> x

fmap2 :: (a -> b -> c) -> f a -> f b
fmap1 g x y = pure g <*> x <*> y

fmap3 :: (a -> b -> c -> d) -> f a -> f b -> f c -> f d
fmap1 g x y z = pure g <*> x <*> y <*> z

.
.
.
```

The class of `functors` that support `pure` and `<*>` functions are called `applicative functors` or short form `applicatives`.

```haskell
class Functor f => Applicative f where
    pure :: a -> f a
    (<*>) :: f (a- -> b) -> f a -> f b
```

#### Examples


