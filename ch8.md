# Chapter 8: Common Collections

## Vectors

### Basics
- **vectors** are like Java lists or C++ vectors
- implemented using generics, **can only store elements of a single type**
- if you know the types needed, **use an enum** to store different types
- **dropping a vector drops all elements** - safe because borrow checker ensures references are only used while vector is valid

### Mutability and Borrowing
- `push`/`pop` requires **`&mut self`** reference
- **cannot hold immutable references** to vector/elements while mutating
- iterating gets **immutable references** to elements by default
- can modify elements while iterating but requires **mutable borrow**
- **cannot push while iterating** (both need mutable borrow)

### Memory Layout and Guarantees
- **always a (pointer, capacity, length) triplet** - field order unspecified
- **pointer never null**, so Vec is **null-pointer optimized**
- if capacity 0, pointer won't point to allocated memory
- **only allocates if `size_of::<T>() * capacity() > 0`**
- **never "small optimizes"** by storing elements on stack
- **never automatically shrinks**, even when empty
- **doesn't guarantee growth strategy**

### Allocation Behavior
- `push`/`insert` **reallocate if `len == capacity`**
- bulk insertion may reallocate even when not necessary
- `Vec::capacity` returns value **`>= requested capacity`**
- can use **uninitialized memory** - removed data won't be erased
- **doesn't guarantee drop order** of elements

### Key Methods
- **`reserve`** - may reserve more to avoid frequent reallocations
- **`reserve_exact`** - reserves minimum capacity
- **`shrink_to_fit`** - may shrink in place or reallocate
- **`truncate`** - keeps first len elements, drops rest
- **`drain`** - returns removed elements instead of dropping
- **`swap_remove`** - O(1) removal, replaces with last element (doesn't preserve order)
- **`retain`** - keep only elements matching predicate
- **`splice`** - replace range with iterator, returns `Splice` (lazy until dropped)

## Strings

### String Types
- **`str`** - string slice (borrowed)
- **`String`** - growable, mutable, owned, UTF-8 encoded
- **`String` is wrapper around `Vec<u8>`**

### String Operations
- **`push_str`** - append string slice (doesn't take ownership)
- **`push`** - append single character
- **`+` operator** - uses `add()`, works with `String` due to deref coercion
- **`format!` macro** - often easier than `+`

### UTF-8 and Indexing
- **no indexing syntax** - characters may be multiple bytes
- **3 ways to view strings**: bytes, scalar values, grapheme clusters
- indexing would be **O(n)** not O(1)
- use **`[]` with ranges** if needed (bounds must align with character boundaries)
- **`.chars()`** for characters, **`.bytes()`** for bytes
- grapheme clusters require external crate

### Internal Representation
- some UTF-8 characters take **multiple bytes**
- Hindi example: 4 grapheme clusters = 6 unicode scalars = 18 bytes
- indexing wouldn't correspond to valid unicode characters

## Hash Maps

### Basics
- **`std::collections::HashMap`**
- **`insert`** to add entries
- **`get`** returns `Option` (provide reference to key)
- iterate with **for loop using reference**
- **not ordered by default**

### Ownership
- **copyable types are copied** into map
- **owned values are moved** into map
- inserting **references** - values must be valid as long as map is valid

### Updating Values
- inserting same key **overwrites by default**
- **`entry().or_insert()`** - equivalent to putIfAbsent, returns value
- updating existing value: get mutable reference via `entry().or_insert()` then dereference to update