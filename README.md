Fizz Buzz TDD Kata in Haskell
=====================

Up and Running
------------
You are familar with counting/dividing game [Fizz Buzz](http://en.wikipedia.org/wiki/Fizz_buzz).

You have Haskell [installed](http://www.haskell.org/platform/)

We will be using the GHCi REPL.

We need a file ``FizzBuzz.hs``

Running it from the command line looks like this

```Shell
% ghci

*Main> :load FizzBuzz
```

You are ready!

Starting with a Test
--------------------

We are using (QuickCheck)[https://github.com/nick8325/quickcheck]

```Haskell
import Test.QuickCheck

```
QuickCheck Tests are checking properties

```Haskell
prop_fizzBuzzForOne m = forAll (elements [1]) $ \m -> fizzBuzz m == "1"
```

Make it Compile
---------------

Compiler says, "You need a fizzBuzz!"

```Haskell
fizzBuzz 1 = ""
```

Compiler says, "I don't know the type of `1`"

```Haskell
fizzBuzz :: Int -> String
fizzBuzz 1 = ""
```

We can run the test

```Bash
*Main> :l FizzBuzz
*Main> quickCheck prop_fizzBuzzForOne
```
Yeah, we have our first failing test!

Make It Pass
-----------

```Haskell
fizzBuzz :: Int -> String
fizzBuzz 1 = "1"
```

We could refactor out the subtle duplication between the test and code. Naw, I just triangulate by expanding my property.

```Haskell
prop_fizzBuzzForPlainNumbers m = forAll (elements [1,2]) $ \m -> fizzBuzz m == show m
prop_fizzBuzzForPlainNumbers m = forAll (elements [1,2,4]) $ \m -> fizzBuzz m == show m
```

```Haskell
fizzBuzz :: Int -> String
fizzBuzz n = show n
```

Test for Numbers Divisible by Three
-----------------------------------

```Haskell
prop_fizzBuzzForNumbersDivBy3 m = forAll (elements [3,6..12]) $ \m -> fizzBuzz m == "fizz"
```

Making it pass

```Haskell
fizzBuzz :: Int -> String
fizzBuzz n
  | 0 == n `mod` 3 = "fizz"
  | otherwise = show n
```

Test for Numbers Divisible by Five
----------------------------------

```Haskell
prop_fizzBuzzForNumbersDivBy5 m = forAll (elements [5,10]) $ \m -> fizzBuzz m == "buzz"
```

Making it pass

```Haskell
fizzBuzz :: Int -> String
fizzBuzz n
  | 0 == n `mod` 3 = "fizz"
  | 0 == n `mod` 5 = "buzz"
  | otherwise = show n
```

Refactor Away
-------------

```Haskell
fizzBuzz :: Int -> String
fizzBuzz n
  | isDivBy 3 n = "fizz"
  | isDivBy 5 n = "buzz"
  | otherwise = show n

isDivBy :: Int -> Int -> Bool
isDivBy x n = 0 == n `mod` x
```

Test for Numbers Divisible by Fifteen
-------------------------------------

```Haskell
prop_fizzBuzzForNumbersDivBy15 m = forAll (elements [15,30,45]) $ \m -> fizzBuzz m == "fizzbuzz"
```

