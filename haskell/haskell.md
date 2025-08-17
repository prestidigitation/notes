## Control Structures

### `if` expressions
```
if <condition> then <true-value> else <false-value>
```

```haskell
evenOrOdd :: Integer -> String
evenOrOdd n = if even n then "The number is even!" else "The number is odd!"
```

### `case-of` expressions
`case-of` is very useful when working with algebraic data types like `Either`. This is because `case-of` can pattern match against expressions, as opposed to matching function arguments.

In fact, `if..then..else` is just syntactic sugar for a very common `case` expression that matches on a `Bool` value.

```haskell
if e1 then e2 else e3
```
is equivalent to
```haskell
case e1 of
  True  -> e2
  False -> e3
```

### How to manage and "track" values in Haskell
__WORK IN PROGRESS__

Passing the accumulator as an argument in the recursive function.

## Function composition and point-free style
Point-free style is when explicit arguments are omitted.
```haskell
-- A function with explicit argument 'x'
myEven :: Int -> Bool
myEven x = not (odd x)

-- The same function rewritten with point-free style
myEven :: Int -> Bool
myEven = not . odd
```
Function composition pipelines the output of one function to the input of another.
`not . odd` is equivalent to `\x -> not (odd x)`.

## Algebraic Data Types
Types are defined this way
```haskell
data TypeName = ConstructorName FieldType FieldType2 | AnotherConstructor FieldType3 | OneMoreCons
```
... or this way if using type variables
```haskell
data TypeName variable = Cons1 variable Type 1 | Cons2 Type2 variable
```
Data type rules:
* They can have one or more constructors.
* Each constructor can have zero or more fields.
* Constructors start with upper case, type variables with lower case.
* Values are handled with pattern matching.

### MISC
When possible, reuse supplied function arguments, such as numbers representing an index. Providing a case where the index is 0 is more clean than defining a helper function.
```haskell
-- Example from Haskell MOOC.
--
-- Ex 4: safe list indexing. Define a function indexDefault so that
--   indexDefault xs i def
-- gets the element at index i in the list xs. If i is not a valid
-- index, def is returned.
--
-- Use only pattern matching and recursion (and the list constructors : and [])
--
-- Examples:
--   indexDefault [True] 1 False         ==>  False
--   indexDefault [10,20,30] 0 7         ==>  10
--   indexDefault [10,20,30] 2 7         ==>  30
--   indexDefault [10,20,30] 3 7         ==>  7
--   indexDefault ["a","b","c"] (-1) "d" ==> "d"

indexDefault :: [a] -> Int -> a -> a
indexDefault []     _ def = def
indexDefault (x:xs) 0 def = x
indexDefault (x:xs) i def = indexDefault xs (i - 1) def
```

---

Don't add explicit True/False return values when there is already a boolean expression in the function!

---

Appending an element to the end of a list using `++` in a recursive function can lead to poor performance. This is because lists in Haskell 
are immutable, so the entire list must be traversed and copied in order to create a list with the appended item at the end.

The most efficient way to build a list in a recursive function is to use prepend elements using the `(:)` operator, and then reversing the 
list at the end. Prepending to linked lists is O(1) time complexity.

```haskell
buildList :: Int -> [Int]
buidList n = reverse (go n [])
  where
    go 0 acc = acc
    go k acc = go (k - 1) (k : acc)
```

## Working with binary
```haskell
data Bin = End | O Bin | I Bin
  deriving (Show, Eq)

-- This function increments a binary number by one.
inc :: Bin -> Bin
inc End   = I End
inc (O b) = I b
inc (I b) = O (inc b)

prettyPrint :: Bin -> String
prettyPrint n = go n ""
  where
    go End acc = acc
    go (O b) acc = go b ('0' : acc)
    go (I b) acc = go b ('1' : acc)

fromBin :: Bin -> Int
fromBin End = 0
fromBin (O b) = 2 * fromBin b
fromBin (I b) = 2 * fromBin b + 1

toBin :: Int -> Bin
toBin 0 = O End
toBin 1 = I End
toBin n
  | remainder == 0 = O (toBin halved)
  | otherwise      = I (toBin halved)
  where
    halved = n `div` 2
    remainder = n `mod` 2
```

---

`all` checks whether all elements of a structure satisfy the predicate.
```haskell 
# Examples
>>> all (> 3) []
True

>>> all (> 3) [1,2,3,4,5]
False
```

---

`zipWith` combines two lists into one list by applying a binary function to each pair of elements.
```haskell
Input: zipWith (+) [1,2,3] [3,2,1]
Output: [4,4,4]

Input: zipWith (\x y -> 2*x + y) [1..4] [5..8]
Output: [7,10,13,16]
```
