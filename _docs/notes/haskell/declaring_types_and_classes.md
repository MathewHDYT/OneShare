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

The simplest way to declare a new type is to introduce a new name for an existing type.

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

Type declarations cannot be recursive. Recursive types can instead be declared using the `data` mechanism.

```haskell
type Tree = (Int, [Tree])
```

Type declarations can be parameterized by other types. Where the same type has to be used on multiple different places.

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

The `data` mechanism functions very similarly to the `enum` in other programming languages.

As with new types, the names of `constructors` must begin with capital letters and the same constructors cannot be used in more than one type.

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

New custom data types are defined by declaring new types and functions that returns values of those types called `data constructors` or `constructors`.

```haskell
data Bool = False | True
-- >>> :t True
-- True :: Bool
-- >>> :t False
-- False :: Bool

data List a = Nil | Cons a (List a)
-- >>> :t Nil
-- Nil :: List a
-- >>> :t Cons
-- Cons :: a -> List a -> List a
```

The `constructors` in a data declaration can also have arguments.

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

Declarations can also be parameterized. 

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

### Type Constructors and Kinds

`Maybe` and `Pair` are not really types in the normal sense. There is a difference between them and types like `Bool` or `Int`.

```haskell
data Maybe a = Nothing | Just a
type Pair a = (a, a)
```

We can provide concrete values of the types `Bool` or `Int`, this however is not possible for `Maybe` and `Pair`.

It is only possilbe to provide concrete values for full `instantiated types`, such as `Maybe Bool` or `Pair Int`.

`Maybe` and `Pair` are so called `Type Constructors` since they constructo one type from another.

This behaviour is expressed using `kinds`. A `kind` is the type of a `"type"`.

```haskell
K ::= * | K -> K
-- *: Pronounced "type" or "start". Kind of ordinary / proper / concrete / baisc types
-- K -> K: Kind of "type constructors" or "type operators"
```

The `kind` can be displayed in GHCi using the `:k` command.

```haskell
data [] a = [] | a : [] a
> :k []
[] :: * -> *

data Either a b = Left a | Right b
> :k Either
Either :: * -> * -> *
```

| Type | Value | Kind |
| ---- | ----- | ---- |
| `Bool` | `True` | * |
| `Char` | `'a'` | * |
| `[a]-> [a]` | `reverse` | * |
| `Maybe Char` | `Just 'a'` | * |
| `a -> Maybe a` | `Just` | * |
| `Maybe` | - | * -> * |
| `(,)` | - | * -> * -> * |
| `Bool -> Bool` | `not` | * |
| `(->) Bool Bool` | `not` | * |
| `(->)` | - | * -> * -> * |
| `(->) Bool` | - | * -> * |
| `(->) a` | - | * -> * |

Only types of `kind` * can have a value.

`Types` are used to prevent the user from making errors at the *value level*, whereas `kinds` are used to prevent the user form making errors at the *type level*.

Not all type expression make sense an instnace of a type class must hae the same `kind` as its class definition.

The `a` in `Eq a` must have `kind` *. Therefore it is not possible to make `Maybe` an instance of `Eq`.

### Newtype declarations

If a new type has a single constructor with a single argument, it can be declared using the `newtype` mechanism.

```haskell
newtype Nat = N Int
type Nat = Int
data Nat = N Int
```

Using `newtype` rather than `type` means that `Nat` and `Int` are different types rather than synonyms.

Meaning they cannot accidentally be mixed up and `newtype` rather than `data` is more efficient, because `newtype` constructors such as `N` do no incur any cost when the program is evaluated. So called `zero-cost abstraction`.

This is possible because they are removed by the compiler once type checking is completed.

### Records

Convenient syntax when data constructors have multiple parameters

```haskell
data Person = Person
    { name :: String
    , age :: Int
    , children :: [Person]
    } deriving (Show)

-- Is equivavlent too

data Person = Person String Int [Person]
              deriving (Show)

-- Selector functions
name :: Person -> String
name (Person n _ _) = n

age :: Person -> Int
age (Person _ a _) = a

children :: Person -> [Person]
children (Person _ _ c) = c
```

This behaviour is more than just syntactic sugar.

```haskell
alice :: Person
alice = Person "Alice" 5 []

bob :: Person
bob = Person {name = "Bob", age = 35, children = [alice]}
-- Construction using field labels

-- >>> alice
-- >>> bob
-- Person {name = "Alice", age = 5, children = []}
-- Person {name = "Bob", age = 35,
--         children = [Person {name = "Alice", age = 5, children = []}]
-- }
-- Derived show contains labels

-- >>> alice {age = age alice + 1}
-- Person {name = "Alice", age = 6, children = []}
-- Updates using field labels
```

### Modules

The main goals of modules are:
- *Organising* programs into multiple files / namespaces
- *Defining visiblity*

and they are:
- A collection of related values, types, classes, etc.
- Name: identifier that starts with a capital letter
- Naming convention for hierarchy: seperated using `'.'` (e.g. `Data.List`, `Data.List.NonEmpty`, `Data.Set`, ...)
- Each module `Dir.Name` must be contained in a file `Dir\Name.hs`

### Recursive types

New types declared using the `data` and `newtype` mechanisms can also be recursive.

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
add m n = int2nat (nat2int m + nat2int n)add' :: Nat -> Nat -> Nat
add' Zero n = n
add' (Succ m) n = Succ (add' m n)

-- add' 2 1
add' (Succ (Succ Zero)) (Suc Zero)
= { applying add' }
Succ (add' (Succ Zero) (Succ Zero))
= { applying add' }
Succ (Succ (add' Zero (Succ Zero)))
= { applying add' }
Succ (Succ (Succ Zero))
-- 1 + 1 + 1 + 0 = 3

````

Furthermore, the data mechanism can be used to declare our own version of the built-in type of lists.

```haskell
data List a = Nil | Cons a (List a)
-- List values of form:
-- Nil / []
-- Cons x xs (x :: a and xs :: List a) / non-empty list

len :: List a -> Int
len Nil = 0
len (Cons _ xs) = 1 + len xs
````

A `binary tree` is also a suitable type which can be represented using recursion.

```haskell
data Tree a = Leaf a | Node (Tree a) a (Tree a)
-- Tree values of form:
-- Leaf / Single point of binary tree
-- Node t x t (t = some value t of type Tree) / Point that splits into two more

t :: Tree Int
t = Node (Node (Leaf 1) 3 (Leaf 4))
          5
         (Node (Leaf 6) 7 (Leaf 9))

{-
Binary tree represented in code above:
           (5)
    (3)           (7)
(1)     (4)   (6)     (9)
-}

occurs :: Eq a => a -> Tree a -> Bool
occurs x (Leaf y) = x == y
occurs x (Node l y r) = x == y || occurs x l || occurs x r
-- Is the value equal to the Leaf
-- Is the value equal to the middle note or the right or left side under the node

flatten :: Trea a -> [a]
flatten (Leaf x) = [x]
flatten (Node l x r) = flatten l ++ [x] ++ flatten r
```

If applying the `flatten` function to a tree gives a sorted list, then the tree is called a `search tree`.

```haskell
-- Where t is the binary tree created above
flatten t = [1, 3, 4, 5, 6, 7, 9]
```

Because the `search tree` is sorted, which of the two subtrees of a node it may occur in can be determined in advance.

```haskell
occurs :: ord a => a -> Tree a -> Bool
occurs x (Leaf y)                 = x == y
occurs x (Node l y r) | x == y    = True
                      | x < y     = occurs x l
                      | otherwise = occurs x r
-- Value is less than the node --> can only occur on the left side
-- Otherwise --> can only occur on the right side
```

This definition is more efficient, because it traverses only one path down the tree, rather than potentially the entire tree.
Additionally, trees can come in many more different forms.

```haskell
data Tree a = Leaf a | Node (Tree a) (Tree a)
-- Tree with data only in the leaves

data Tree a = Leaf | Node (Tree a) a (Tree a)
-- Tree with data only in the nodes

data Tree a b = Leaf a | Node (Tree a b) b (Tree a b)
-- Tree with different types in their leaves and nodes

data Tree a = Node a [Tree a]
-- Tree with a list of subtrees, Node with empty list of subtrees can play role of a leaf
```

### Polymorphism

The term `polymorphism` is used to denote a number of fundamentelly different variants.
- *Ad-hoc Polymorphism*: Function with the same name denotes different implementations (function overloading, Interfaces)
- *Parametric Polymorphism*: Code written to work with many possible types (Object-Oriented: Generics)
- *Subtype Polymorphism*: One type (subtype) can be substitued for another (supertype)

```haskell
-- Parametric Polymorphism, (a is generic)
data Tree a = Leaf | Node (Tree a) a (Tree a)
```

### Class and instance declarations

A new class can be declared using the `class` mechanism.

```haskell
class Eq a where
    (==), (/=) :: a -> a -> Bool

    x /= y = not (x == y)
```

Specifies that for a `type a` to be an instance of the `class Eq`, it must support equality operators of the specified type.
Inequality is not needed, because a `default definition` has already been included for the `/=` operator.

```haskell
instance Eq Bool where
    False == False = True
    True  == True  = True
    _     == _     = False
```

Only types that are declared using the `data` or `newtype` mechanism can be made into instances of classes.
Note that `default definitions` can also be overridden in instance declarations.

Classes can also be extended to form new classes.

```haskell
class Eq a => Ord a where
    (<), (<=), (>), (>=) :: a -> a -> Bool
    min, max             :: a -> a -> a

    min x y | x >= y    = x
            | otherwise = y

    max x y | y <= y    = y
            | otherwise = x

instance Ord Bool where
    False < True = True
    _     < _    = False

    b <= c = (b < c) || (b == c)
    b > c  = c < b
    b >= c = c <= b
```

### Derived instances

New types can be made into instances of a number of built-in classes using the `deriving` mechanism.

```haskell
data Bool = False | True
            deriving (Eq, Ord, Show, Read)

> False == False
True

-- Ordering is determined by their positions in its declaration (data Bool = False | True),
-- meaning False is smaller than True
> False < True
True

> show False
"False"

-- :: is required to resolve the type of the result
> read "False" :: Bool
False
```

In case of constructors with arguments, the types of the arguments must also be instances of any `derived class`.

```haskell
-- For Shape to be an equality type, Float must also be an equality type
data Shape = Circle Float | Rect Float Float
             deriving (Eq, Ord, Show, Read)

-- For Maybe to be an equality type, a must also be an equality type,
-- which then becomes a class constraint on the parameter
data Maybe a = Nothing | Just a
               deriving (Eq, Ord, Show, Read)
```

Values build using constructors with arguments are ordered lexicographically.

```haskell
> Rect 1.0 4.0 < Rect 2.0 3.0
True

> Rect 1.0 4.0 < Rect 1.0 3.0
False
```
