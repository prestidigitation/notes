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

Don't add explicit True/False return values when there is already a boolean expression in the function!