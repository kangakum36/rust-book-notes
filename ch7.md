# Chapter 7: Managing Growing Projects with Packages, Crates, and Modules

## Project Organization Overview
Rust has builtin ways to manage projects:
- **packages** - let you build, test and share crates
- **crates** - tree of modules that produces a library or executable
- **modules and use** - let you control organization, scope, and privacy of paths
- **paths** - way of naming an item, such as a struct, function, or module

## Crates and Packages
- **crate** is the smallest amount of code the rust compiler will compile at one time
- can be binary or library
- **package** is a bundle of one or more crates that provide a set of functionality
- package has a `Cargo.toml` file that describes how to build
- package can have many binary crates but **at most one library crate**
- package must have **at least one crate**
- can create multiple binary crates by placing files in the `src/bin` directory

## Modules
### Module Resolution
Compiler first looks in the **crate root file** for code to compile (`lib.rs` or `main.rs`)

In the crate root file, can declare new modules. Compiler will look for the code:
- inline
- in the file `src/<mod_name>.rs`
- in the file `src/<mod_name>/mod.rs`

Can declare **submodules** in files other than the crate root:
- inline
- in the file `src/<mod_name>/<submod_name>.rs`
- in the file `src/<mod_name>/<submod_name>/mod.rs`

### Module System
- `src/main.rs` and `src/lib.rs` are **crate roots**, contents form a module called `crate`
- module system is similar to a filesystem, crate roots form the "root" module
- can use absolute or relative paths to find items in the module tree
- **generally prefer absolute paths** to avoid having changes when moving code

### Privacy Rules
- **code in a module is private by default**
- **parent modules cannot use code inside child modules**, but child modules can use code inside parent modules
- can refer to private sibling modules, but can't access their private contents
- **even in public modules, the contents are still private by default!**

### Path Construction
- once a module is part of a crate, can refer to code from anywhere else in the same crate
- follow module paths: `crate::<mod_name>::<submod_name>::<type>`
- can construct relative paths that start at parent module with **`super`**

## Public Items
- **`pub` before struct definition** designates them as public, but the fields will be private
- for structs without public fields, implementation needs to have a public associated function that constructs an instance
- **if we make an enum public, then all of its variants are public** (by default all enum variants are public)

## Use Keyword
- **`use` keyword** helps create a shortcut to a path, like a sym link in a filesystem
- `use` only creates the shortcut for the scope in which the use occurs
- should **bring the parent module into scope** with use and then call functions with `parent::function`
- when bringing in structs, enums, and other items, **idiomatic to specify the full path**

### Use Best Practices
- if bringing items with the same name into scope, bring the parent modules into scope
- alternatively, can use the **`as` keyword** to rename items
- **`pub use`** enables re-exporting - code outside scope can refer to the name as if it had been defined in that scope
- with `pub use`, we can write code with one structure but expose a different one

### Use Syntax
- can use **nested paths** to clean up large use lists: `use std::{cmp::Ordering, io}`
- can use the **glob operator `*`**, but like Java * imports, best to avoid this

## External Packages
- to use an external package, add a line to `Cargo.toml`
- tells Cargo to download the package and dependencies from crates.io
- to bring definitions into scope, add a `use` line starting with the name of the crate

## File Organization
- when modules get large, might want to move definitions into a separate file
- only need to **load a file using a `mod` declaration once** in the module tree
- other files should refer to the loaded code using a path to the declaration
- **`mod` is not an include operation**
- `src/garden/vegetables.rs` vs `src/garden/vegetables/mod.rs` - latter is older style and confusing

## Best Practices
- if creating a package with both a binary and library crate, **best practice is to have just enough code in binary crate** to start an executable that calls code defined in the library crate