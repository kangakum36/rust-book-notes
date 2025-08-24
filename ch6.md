# Chapter 6: Enums and Pattern Matching

## Enums
- **enums** are similar to other languages, give you a way to say a value is one of a possible set
- example is ip addresses (v4 or v6)
- name of each enum we define is a function that constructs the enum
- can derive traits on enums too
- can also have `impl`s for enums
- **enums can have fields** as well, so instances of an ip address enum can have an associated address
- in order to access these fields, seems like we need to use a match statement
- you can put any data inside an enum

## Option Enum
- basically java optional with `Some` and `None`
- even has generic `<T>`
- **option is better than null** because compiler forces you to do the check
- option has a lot of ways to get the value out of a `Some` variant:

### Option Methods
- `match` - pattern matching
- explicit check
- `as_slice` - return slice of contained value
- `unwrap` - will panic if none, return if some
- `expect` - like orElseThrow, can provide custom message
- `unwrap_or_else` - will unwrap or return a value from a closure
- `unwrap_or_default` - will unwrap or return default value for the type (must implement Default)
- `map` - Option<T> to Option<U>
- `inspect` - calls a function with &T if some, and returns the option
- `ok_or` - maps some to Ok and None to Err, converts to Result
- `or` - returns the option or another option
- `get_or_insert` - for a mutable value into an option
- `take` - take the value out of the option, leaving None in its place (mut)
- `transpose` - option of result into a result of option
- `flatten` - option of option into an option

## Match Control Flow
- the compiler confirms **all possible cases are handled**
- as we discovered previously, **match arms can bind to values**
- of course, we can match `Option<T>`
- if you name the catch all value, you can use it (to do some default behavior on it)
- if you use `_`, your default behavior can't use the value
- to do nothing with an arm, use the unit value `()`

## If Let Syntax
- combines `if` and `let` to handle values that match one pattern while ignoring the rest
- code in the `if let` block only runs if the value matches the pattern
- **match ensures you aren't forgetting cases**, tradeoff between conciseness and exhaustive checking
- can include an `else` block like the `_` block in match

## Let...Else
- `let...else` can be useful to define another value if a first value is an specific enum variant or else define a default
- high level rule with `if let` and `let...else` is that if a program has logic that is too complex for match, it might be worth considering `if let` and/or `let...else`