---
layout: default
title: Recursive functions
parent: Haskell
nav_order: 6
grand_parent: Notes
---

## Recursive functions
Relevant content from Chapter 6 of [Programming in Haskell 2nd Edition](https://www.cambridge.org/us/academic/subjects/computer-science/programming-languages-and-applied-logic/programming-haskell-2nd-edition)

### Basic concepts

Many functions can naturally be defined in terms of other functions.

```haskell
fac :: Int -> Int
fac n = product [1..n]
```

It is permissible to define functions in terms of themselves, in which case the function is called `recursive`.

```haskell
fac :: Int -> Int
fac 0 = 1
fac n = n * fac (n - 1)
```

The first equation states that the factorial of zero is one, called a `base case`.
Whereas the second equation states that the factorial of any other number is given by the product of that number and the factorial of its predecessor, called a `recursive case`.

```haskell
fac 3
= { applying fac }
3 * fac 2
= { applying fac }
3 * (2 * fac 1)
= { applying fac }
3 * (2 * (1 * fac 0))
= { applying fac }
3 * (2 * (1 * 1))
= { applying * }
6
```

Note that the `fac` function does not loop forever, because each application decreases the integer argument by one, until it eventually reaches zero, at which point the recursion stops.
Returning one as the factorial of zero is appropriate, because one is the identity for multiplication.

```haskell
1 * x = x
x * 1 = x
-- For any integer x
```

```haskell
(*) :: Int -> Int -> Int
m * 0 = 0
m * n = m + (m * (n - 1))


4 * 3
= { applying * }
4 + (4 * 2)
= { applying * }
4 + (4  + (4 * 1))
= { applying * }
4 + (4  + (4 + (4 * 0)))
= { applying * }
4 + (4  + (4 + 0))
= { applying + }
12
```

### Recursion on lists

Recursion is not restricted to functions on integers, but can be used for lists as well.

```haskell
prodcut :: Num a => [a] -> a
product [] = 1
product (n:ns) = n * product ns
```

The first equation states that the product of the empty lists of number is one.
Whereas the second equation states that the product of a non-empty lists is given by multiplying the first number and the product of the remaining list.

```haskell
product [2, 3, 4]
= { applying product }
2 * product [3, 4]
= { applying product }
2 * (3 * product [4])
= { applying product }
2 * (3 * (4 * product []))
= { applying product }
2 * (3 * (4 * 1))
= { applying * }
24
```

Recall that lists are actually constructed one element at a time using the `cons` operator.

```haskell
length :: [a] -> Int
length [] = 0
length (_:xs) = 1 + length xs
```

The length of the empty list is zero, and the length of any non-empty list is 1 in addition to the length of its tail.
Note that the use of the wildcard pattern `_` in the `recursive case`, which reflects the fact that calculating the length of a list does not depend upon the values of its elements.

```haskell
reverse :: [a] -> [a]
reverse [] = []
reverse (x:xs) = reverse xs ++ [x]
```

The reverse of the empty list is simply the empty list, and the reverse of a non-empty list is given by appending `++` the reverse of its tail and a singleton list compromising the head of the list.

```haskell
reverse [1, 2, 3]
= { applying reverse }
reverse [2, 3] ++ [1]
= { applying reverse }
(reverse [3] ++ [2])++ [1]
= { applying reverse }
((reverse [] ++ [3]) ++ [2])++ [1]
= { applying reverse }
(([] ++ [3]) ++ [2])++ [1]
= { applying * }
[3, 2, 1]
```

The append operator `++` can itself be defined using recursion on its first argument.

```haskell
(++) :: [a] -> [a] -> [a]
[] ++ ys = ys
(x:xs) ++ ys = x : (xs ++ ys)
```

```haskell
[1, 2, 3] ++ [4, 5]
= { applying ++ }
1 : ([2, 3] ++ [4, 5])
= { applying ++ }
1 : (2 : ([3] ++ [4, 5]))
= { applying ++ }
1 : (2 : (3 : ([] ++ [4, 5])))
= { applying ++ }
1 : (2 : (3 : [4, 5]))
= { applying ++ }
[1, 2, 3, 4, 5]
```

The `recursive` definition for `++` formalizes the idea that two lists can be appended by copying elements from the first list until it is exhausted, at which point the second list is joined on at the end.

```haskell
insert :: Ord a => a -> [a] -> [a]
insert x [] = [x]
inser x (y:ys) | x <= y = x : y : ys
               | otherwise = y : insert x ys
```

Inserting a new element into an empty list gives a singleton list,
while for a non-empty list the result depends upon the ordering of the new element x and the head of the list y.

```haskell
insert 3 [1, 2, 4, 5]
= { applying insert }
1 : insert 3 [2, 4, 5]
= { applying insert }
1 : (2 : (insert 3 [4, 5]))
= { applying insert }
1 : (2 : (3 : (4 : [5])))
= { list notation }
[1, 2, 3, 4, 5]
```

A function that implements `insertion sort`, in which the empty list is already sorted, and a non-empty list is sorted by inserting its head into the list that results from sorting its tail.

```haskell
isort :: Ord a => [a] -> [a]
isort [] = []
isort (x:xs) = insert x (isort xs)
```

```haskell
isort [3, 2, 1, 4]
= { applying isort }
insert 3 (isort [2, 1, 4])
= { applying isort }
insert 3 (insert 2 (isort [1, 4]))
= { applying isort }
insert 3 (insert 2 (insert 1 (isort [4])))
= { applying isort }
insert 3 (insert 2 (insert 1 (insert 4 (isort []))))
= { applying insert }
insert 3 (insert 2 (insert 1 (insert 4 [])))
= { applying insert }
insert 3 (insert 2 (insert 1 [4]))
= { applying insert }
insert 3 (insert 2 [1, 4])
= { applying insert }
insert 3 [1, 2, 4]
= { applying insert }
[1, 2, 3, 4]
```

###  Multiple arguments

Functions with multiple arguments can also be defined using recursion on more than one argument at the same time.

```haskell
zip :: [a] -> [b] -> [(a, b)]
zip [] _ = []
zip _ [] = []
zip (x:xs) (y:ys) = (x, y) : zip xs ys
```

```haskell
zip ['a', 'b', 'c'] [1, 2, 3, 4]
= { applying zip }
('a', 1) : (zip ['b', 'c'] [2, 3, 4])
= { applying zip }
('a', 1) : (('b', 2) : (zip ['c'] [3, 4]))
= { applying zip }
('a', 1) : (('b', 2) : (('c', 3) : (zip [] [4])))
= { applying zip }
('a', 1) : (('b', 2) : (('c', 3) : []))
= { list notation }
[('a', 1), ('b', 2), ('c', 3)]
```

Note that two `base cases` are required in the definition of `zip`, because either of the two argument lists may be empty.

```haskell
drop :: Int ->  [a] -> [a]
drop 0 xs = xs
drop _  [] = []
drop n (_:xs) = drop (n - 1) xs
```

Two `base cases` are requires, one for removing zero elements, and one for attempting to remove elements from an empty list.

### Multiple recursion

Functions can also be defined using `multiple recursion`, in which a function is applied more than once in its own definition.

```haskell
fib :: Int -> Int
fib 0 = 0
fib 1 = 1
fib n = fib (n - 2) + fib (n - 1)
```

```haskell
qsort :: Ord a => [a] -> [a]
qsort [] = []
qsort (x:xs) = qsort smaller ++ [x] ++ qsort larger
               where
                 smaller = [a | a <- xs, a <= x]
                 larger = [b | b <- xs, b > x]
```

The empty list is already sorted, and any non-empty list can be sorted by placing its head between two lists result from sorting those elements of its tails that are `smaller` and `larger` than the head.

### Mutual recursion

Functions can also be defined using `mutual recursion`, in which two or more functions are all defined recursively in terms of each other.

```haskell
even :: Int -> Bool
even 0 = True
even n = odd (n - 1)

odd :: Int -> Bool
odd 0 = False
odd n = even (n - 1)
```

Zero is even but not odd, and any other number is even if its predecessor is odd, and odd if its predecessor is even.

```haskell
even 4
= { applying even }
odd 3
= { applying odd }
even 2
= { applying even }
odd 1
= { applying odd }
even 0
= { applying even }
True
```
