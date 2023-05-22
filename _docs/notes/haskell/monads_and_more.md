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

Hence, `Maybe` is a functor and therefore supports `fmap`. It is straightforward to make this type into an applicative functor

```haskell
instance Applicative Maybe where
    -- pure :: a -> Maybe a
    pure = Just
    -- (<*>) :: Maybe (a -> b) -> Maybe a -> Maybe b
    Nothing <*> _ = Nothing
    (Just g) <*> mx = fmap g mx
```

The function `pure` transforms a value into a successful result, while the operator `<*>` applies a function that may fail to an argument that produces a result that may fail.

```haskell
> pure (+ 1) <*> Just 1
Just 2

> pure (+) <*> Just 1 <*> Just 2
Just 3

> pure (+) <*> Nothing <*> Just 2
Nothing
```

In this manner, the applicative style `Maybe` supports a form of `exceptional` programming, applying pure function to argument that may fail, without the need to manage the propagation of failures ourselves.

```haskell
instance Applicative [] where
    -- pure :: a -> [a]
    pure x = [x]
    -- (<*>) :: [a -> b] -> [a] -> [b]
    gs <*> xs = [g x | g <- gs, x <- xs]
```

The function `pure` transform a value into a `singleton list`, while `<*>` takes a list of functions and a list of arguments and applies each function to each argument in turn.

```haskell
> pure (+ 1) <*> [1, 2, 3]
[2, 3, 4]

> pure (+) <*> [1] <*> [2]
[3]

> pure (*) <*> [1, 2] <*> [3, 4]
[3, 4, 6, 8]
```

They key to view the type `[a]` as a generalization of `Maybe a` that permits multiple results in the case of success.
More precisely, the empty lists can be though of as representing failure, and a non-empty lists as representing all the possible ways in which a result may succeed.

Hence, in the last example there are two possible values for the first argument `[1, 2]` and two possible values for the second one `[3, 4]`.
Which gives four possible results for the multiplication `[3, 4, 6, 8]`.

```haskell
prods :: [Int] -> [Int] -> [Int]
prods xs ys = [x * y | x <- xs, y <- ys]

-- Applicative definition, avoids naming intermediate results
prods' :: [Int] -> [Int] -> [Int]
prods' xs ys = pure (*) <*> xs <*> ys
```

Applicative style for list supports a form of `non-deterministic` programming in which we can apply pure functions to multivalued arguments without the need to manage selection of values or propagation of failure.

```haskell
instance Applicative IO where
    -- pure :: a -> IO a
    pure = return
    -- (<*>) :: IO (a -> b) -> IO a -> IO b
    mg <*> mx = do {g <- mg; x <- mx; return (g x)}
```

Here the function `pure` is given by the `return` function for the `IO` type, and `<*>` applies an impure function to an impure argument and gives an impure result.

```haskell
getChars :: Int -> IO String
getChars 0 = return []
getChars n = pure (:) <*> getChar <*> getChars (n - 1)
```

In the base case we simply return the empty list, and in the recursive case we apply the cons operator to the result of reading the first character and the remaining list of characters.
Applicative style for `IO` supports a form of `interactive` programming in which we can apply pure functions to impure arguments without the need to manage the sequencing of actions or the extraction of result values.

#### Effectful programming

The common theme between all `Applicative` instances is that they all concert programming with `effects`.
In each case, the instance provides an operator `<*>` that allows us to write programs in a familiar application style in which functions are applied to arguments, with one key difference:
the arguments are no longer just plain values but may also have effects (`Maybe`, `IO`, `[]`).

```haskell
sequenceA :: Applicative f => [f a] -> f [a]
sequenceA [] = pure []
sequenceA (x:xs) = pure (:) <*> x <*> sequenceA xs
```

Transforms a list of applicative actions into a single such action that returns a list of result values and captures a common pattern of applicative programming.

```haskell
getChars :: Int -> IO String
getChars = sequenceA (replicate n getChar)
```

#### Applicative laws

In addition to providing the function `pure` and `<*>`, applicative functors are also required to satisfy four equational laws.

```haskell
pure id <*> x = x
pure (g x) = pure g <*> pure x
x <*> pure y = pure (\g -> g y) <*> x
x <*> (y <*> z) = (pure (.) <*> x <*> y) <*> z
```

1. `pure` preserves the identity function, applying `pure` to a function returns an applicative version of the `id` function
2. `pure` preserves function application, applying `pure` over normal function application results in an applicative application
3. When applying effectful function `<*>` to a pure argument the order of evaluation does not matter
4. `<*>` is associative, meaning rearranging the order does not influence the result

There also exits an infix version of `fmap`, defined by `g <$> x = fmap g x`, which allows for a different formulation of applicative style.

```haskell
pure g <*> x1 <*> x2 <*> ... <*> xn
g <$> x1 <*> x2 <*> ... <*> xn
```

#### Evaluation

Notice the similarity between `$` and `<*>`.

```haskell
($) :: (a -> b) -> a -> b
f $ x = f x

(+) $ 11 $ 31
= ((+) 11) $ 31
= ((+) 11 31)
= 42

(<*>) :: f (a -> b) -> f a -> f b

pure (+) <*> Just 11 <*> Just 31
= Just (+) <*> Just 11 <*> Just 31
= Just ((+) 11) <*> Just 31
= Just ((+) 11 31)
= Just 42
```

The following derivation illustrates how applicatives can be evaluated.

```haskell
pure (+) <*> [1, 2] <*> [3, 4]
= [(+)] <*> [1, 2] <*> [3, 4]
= [(+) 1, (+) 2] <*> [3, 4]
= [(+) 1 3, (+) 1 4, (+) 2 3, (+) 2 4]
= [4, 5, 5, 6]
```

### Monads

Type of expressions that are built up from integer values using a division operator.

```haskell
data Expr = Val Int | Div Expr Expr

eval :: Expr -> Int
eval (Val n) = n
eval (Div x y) = eval x `div` eval y
```

However, this function does not take the possibility of division by zero in account, and will produce an error.

```haskell
> eval (Div (Val 1) (Val 0))
*** Exception: divide by zero
```

To address this the `Maybe` type to define a safe version of division that returns `Nothing` when the second argument is zero can be used.

```haskell
safediv :: Int -> Int -> Maybe Int
safediv _ 0 = Nothing
safediv n m = Just (n `div` m)
```

Then we can modify our evaluator to explicitly handle the possibility of failure.

```haskell
eval :: Expr -> Maybe Int
eval (Val n) = Just n
eval (Div x y) = case eval x of
                    Nothing -> Nothing
                    Just n -> case eval y of
                                Nothing -> Nothing
                                Just m -> safediv n m

> eval (Div (Val 1) (Val 0))
Nothing
```

This resolved the division by zero issue, but is rather verbose.
To mitigate that we can use the fact that `Maybe` is applicative and redefine `eval` in an applicative style.

```haskell
eval :: Expr -> Maybe Int
eval (Val n) = pure n
eval (Div x y) = pure safediv <*> eval x <*> eval y
```

However, this definition is not type correct. Because the function `safediv` has type `Int -> Int -> Maybe Int`, whereas in the above context a function of type `Int -> Int -> Int` is required.

Replacing `pure safediv` by a custom defined function would not help either because this function would need to have type `Maybe (Int -> Int -> Int)`, which does not provide any means to indicate failure, when the second argument is zero.

Concluding the function `eval` does not fin the pattern of effectful programming captured by applicative functors, because it restricts applying pure functions to effectful arguments, this is not matched because the function `safediv` is not pure because it by itself may fail.

Abstracting out the pattern of mapping `Nothing` to itself and `Just x` to some result depending on x.

```haskell
(>>=) :: Maybe a -> (a -> Maybe b) -> Maybe b
mx >>= f = case mx of
             Nothing -> Nothing
             Just x -> f x
```

`>>=` takes an argument of type `a` that may fail and a function of type `a -> b` whose result may fail, and returns a result of type `b` that may fail.
If the argument fails the failure is propagated, otherwise we apply the function to the resulting value.

Often called `bind`, because the second argument binds the result of the first.

```haskell
eval :: Expr -> Maybe Int
eval (Val n) = Just n
eval (Div x y) = eval x >>= \n ->
                 eval y >>= \m ->
                 safediv n m
```

First we evaluate `x` and call its result value `n`, then evaluate `y` and call its result value `m`. Finally, we combine two results by applying `safediv`.

A typical expression built using the `>>=` operator has the following structure.

```haskell
m1 >>= \x1 ->
m2 >>= \x2 ->
.
.
.
mn >>= \xn ->
f x1 x2 ... xn
```

We evaluate each of the expression `m1 ... mn` in turn, and then combine their result values `x1 ... xn` by applying the function `f`.
The expression only succeeds if every component `mi` in the sequence succeeds.

```haskell
do x1 <- m1
   x2 <- m2
   .
   .
   .
   xn <- mn
   f x1 x2 ... xn
```

Each item in the sequence must begin in the same column, and `xi <- mi` can be abbreviated with `mi` if the value `xi` is not required.

```haskell
eval :: Expr -> Maybe Int
eval (Val n) = Just n
eval (Div x y) = do n <- eval x
                    m <- eval y
                    safediv n m
```

The `do` notation can be used with any applicative type that forms a `monad`.

```haskell
class Applicative m => Monad m where
    return :: a -> m a
    -- Defaul definition to pure, can be overwritten
    return = pure
    (>>=) :: m a -> (a -> m b) -> m b
```

A monad is an applicative type `m` that supports `return` and `>>=` function of the specified type.

#### Examples

The standard prelude bind, defines the bind operator using pattern matching rather than `case analysis`.

```haskell
instance Monad Maybe where
    -- (>>=) :: Maybe a -> (a -> Maybe b) -> Maybe b
    Nothing >>= _ = Nothing
    (Just x) >>= f = f x

instance Monad [] where
    -- (>>=) :: Maybe a -> (a -> Maybe b) -> Maybe b
    (x:xs) >>= f = [y | x <- xs, y <- f x]

pairs :: [a] -> [b] -> [(a, b)]
pairs xs ys = do x <- xs
                 y <- ys
                 -- Could be pure (x, y), but the convention is to use return
                 return (x, y)

> pairs [1, 2] [3, 4]
[(1, 3), (1, 4), (2, 3), (2, 4)]

-- Notice similairty to definition using list comprehension
pairs :: [a] -> [b] -> [(a, b)]
pairs xs ys = [(x, y) | x <- xs, y <- ys]

-- Definition is build-in to the language
instance Monad IO where
    -- return :: a -> m a
    return x = ...
    -- (>>=) :: IO a -> (a -> IO b) -> IO b
    mx >>= f = ...
```

The `let` mechanism is similar to the `where` mechanism, expect that it allows local definitions to be made at the level of expressions rather than at the level of function definitions.

```haskell
f x = y
   where y = ... x ...

f x =
   let y = ... x ...
   in  y
```

#### Monad laws

In addition to providing the function `return` and `>>=`, monadic functors are also required to satisfy three equational laws.

```haskell
return x >>= f = f x
mx >>= return = mx
(mx >>= f) >>= g = mx >>= (\x -> (f >>= g))
```

1. `return` a value and then feeding it into a monadic function, should give the same result as applying the function to the value
2. Giving result of monadic computation into `return`, should give back the same result as performing calculation
3. `return` preserves the identity for the `>>=` operator, stated by the previous two rules combined
4. `>>=` is associative, meaning rearranging the order does not influence the result

#### Composing `Maybe`effects

The following code defines and operator `(>>=)` that we can use to compose two `Maybe` effects into one.

```haskell
(>>=) :: Maybe Int -> (Int -> Maybe Int) -> Maybe Int
safediv 8 2 (>>=) (\x -> safediv n 2)
```

More generally the type is.

```haskell
(>>=) :: Maybe a -> (a -> Maybe b) -> Maybe b
(>>=) :: m a -> (a -> m b) -> m b
```

#### Evaluation

The following derivation illustrates how monads can be evaluated.

```haskell
safediv :: Int -> Int -> Maybe Int
safediv _ 0 = Nothing
safediv l r = Just (l `div` r)

do n <- pure 10
   m <- pure 2
   safediv n m
= pure 10 >>= (\n -> (pure 2 >>= (\m -> safediv n m))) -- (do syntax with explicit λ parentheses)
= Just 10 >>= (\n -> (pure 2 >>= (\m -> safediv n m))) -- (definition of pure)
= (\n -> (pure 2 >>= (\m -> safediv n m))) 10
 -- (definition of >>=)
= pure 2 >>= (\m -> safediv 10 m) -- (function application)
= Just 2 >>= (\m -> safediv 10 m) -- (definition of pure)
= (\m -> safediv 10 m) 2 -- (definition of >>=)
= safediv 10 2 -- (function application)
= Just (10 `div` 2) -- (definition of safediv)
= Just 5 -- (definition of div)
```

#### Kleisli Arrow `(<=<)`

Is analagous to normal function application, exepct that it works on monadic functions.

```haskell
(.) :: (b -> c) -> (a -> b) -> a -> c
(<=<) :: Monad m => (b -> m c) -> (a -> m b) -> a -> m c
(f <=< g) x = g x >>= f 
```

Monad type class laws expressed in terms of the Kleisli arrows.

```haskell
-- Left identity
return <=< f == f
-- Right identity
f <=< return == f
-- Associativity
(f <=< g) <=< h == f <=< (g <=< h)
```

### Overview

| | **APPLICATION OPERATOR** | **TYPE OF APPLICATION OPERATOR** | **FUNCTION** | **ARGUMENT** | **RESULT** |
| --- | --- | --- | --- | --- | --- |
| Pure function application | `juxtapose`, `($)` | `(a -> b) -> a -> b` | pure | pure | pure |
| Functor | `fmap`, `(<$>)` | `(a -> b) -> f a -> f b` | pure | effectual | effectual |
| Applicative functor | `(<*>)` | `f (a -> b) -> f a -> f b` | pure | effectual | effectual |
| Monad | `(>>=)` | `m a -> (a -> m b) -> m b` | effectual | effectual | effectual |

### Defining Applicatives and Functors in terms of Monads

This is possible, since all monads are applicative functors which in turn are all functors. See this [StackOverflow thread](https://stackoverflow.com/questions/19635265/is-it-better-to-define-functor-in-terms-of-applicative-in-terms-of-monad-or-vic) for more information.

```haskell
instance Applicative MyMonadType where
    pure = return
    mf <*> mx = do
    f <- mf
    x <- mx
    return (f x)

instance Functor MyApplicativeType where
    fmap = fmap f x = pure f <*> x
```
