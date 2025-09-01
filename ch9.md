# Chapter 9: Error Handling

## Error Handling Philosophy
- Rust has **recoverable** and **unrecoverable** errors
- Similar to Java's checked/unchecked exceptions, but **Rust doesn't have exceptions**
- **`Result<T, E>`** for recoverable errors
- **`panic!` macro** for unrecoverable errors that immediately stop execution

## Panic! Macro

### Behavior
- Can **explicitly call** or take an action that causes code to panic
- By default, will **unwind the stack** but can configure to **abort instead**
- Get **backtrace on panic** with `RUST_BACKTRACE=1` environment variable

## Result Type

### Basics
- **`enum Result<T, E>`** with variants `Ok(T)` and `Err(E)`
- `T` and `E` are generic parameters

### Handling Results
- **`match` arms** to handle both cases
- **`unwrap_or_else`** with closure for less verbose handling
- **`unwrap`** - shortcut that panics on error
- **`expect`** - like unwrap but lets you choose the error message

### The ? Operator
- **`?` operator** can return `Result` types from functions
- If `Ok`, value gets returned and program continues
- If `Err`, the `Err` is returned early from function
- **`?` converts errors** to desired type if they implement `From` trait
- **works on `Option`** too - `None` returned early, `Some` assigned

## When to Panic vs Return Result

### When to Panic
- **Examples, prototype code, and tests**
- **When you have more information than the compiler** (e.g., hardcoded valid IP string)
- **Code could end up in bad state** - broken contract/invariant that is unexpected
- **Continuing execution would be insecure or harmful**
- **External code returns invalid state** that you can't fix

### When to Return Result
- **Failure is expected** (e.g., parser with malformed data, HTTP rate limit)
- Caller should decide how to handle the error

## Custom Types for Validation
- **Create types that guarantee valid state**
- Do validation checks in the **`new` function**
- Keep **internal state private**
- Ensures invalid states cannot be constructed