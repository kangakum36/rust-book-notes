# Chapter 5: Using Structs

## Struct Basics
- can't mark specific fields of struct instance as mutable, entire instance must be mutable
- **struct update** to create struct from another struct - always moves non copyable types

## Struct Types
### Tuple Structs
- added meaning of struct name but no named fields
- like points in xy plane, etc. have to name type when destructuring

### Unit Structs
- no data, only traits implemented on them
- for example, type that is always equal to any other type

## References in Structs
- in order for a struct to contain references, we must use lifetimes (don't know about these yet)

## Derived Traits
- **derived traits** on structs allow us to do useful things
- `#[derive(Debug)]` allows us to use with println

## Methods
- methods are defined within the context of a struct, enum, or trait object
- first param is always `self`
- `&self` is actually short for `self: &Self`
- within an `impl` block, `Self` is an alias for the type that the impl block is for
- can also take ownership with `self`, or mutably borrow with `&mut self`

### Method Features
- **getters** not impl by default, but can alias the type name
- there is no `->` op in rust, auto references and derefs
- if you call a method, rust automatically adds `&`, `&mut`, or `*` so the object matches the method signature

## Associated Functions
- all functions in an `impl` block are **associated functions**
- can have associated functions that are not methods -> they don't take a self parameter
- Example is `String::from`
- can have multiple impl blocks - not any reason to do it yet, ch10 promises answers