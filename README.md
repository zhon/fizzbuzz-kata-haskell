Fizz Buzz TDD Kata in Haskell
=====================

Up and Running
------------
You are familar with counting/dividing game (Fizz Buzz)[http://en.wikipedia.org/wiki/Fizz_buzz].

You have Haskell (installed)[http://www.haskell.org/platform/]

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
prop_fizzBuzzForOne m = forAll (choose (1,1)) $ \m -> fizzBuzz m == "1"
```

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

```Haskell
fizzBuzz :: Int -> String
fizzBuzz 1 = "1"
```

We could refactor out the subtle duplication between the test and code. Naw, I just triangulate by expanding my property.

```Haskell
prop_fizzBuzzForPlainNumbers m = forAll (choose (1,2)) $ \m -> fizzBuzz m == show m
```

```Haskell
fizzBuzz :: Int -> String
fizzBuzz n = show n
```
