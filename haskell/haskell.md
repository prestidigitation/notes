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
