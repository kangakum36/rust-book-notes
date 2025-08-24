# Chapter 4: Understanding Ownership

## Memory Management
- **stack** - LIFO, values usually in cache, fast to access
- **heap** - everything is a pointer, need allocations, need to follow

## Ownership Rules
- every value has an **owner**
- when variable goes out of scope `drop` is called
- variables of types that implement `Copy` are trivially copy-assigned, others are move-assigned
- can't use a moved-from variable
- `clone` will deep copy instead of moving

## Functions and Ownership
- functions always take ownership, return can transfer ownership

## References and Borrowing
- **reference** is an address we can follow to data stored at that address
- references are guaranteed to be valid, unlike pointers
- visual is same as a pointer to the variable
- **borrowing** = action of creating a reference

### Borrowing Rules
- can't modify something you have borrowed, unless its a mutable borrow
- **mutable reference rule** - if have a mutable ref, must be the only ref
- can't have mutable and immutable ref
- can't have mutable and mutable ref
- prevents data races

### Dangling References
- compiler guarantees that refs will not be dangling
- can't use a ref to a value that has gone out of scope
- ex, can't return ref to a function-local variable

## String Slices
- reference to contiguous seq of elements in the string
- ptr to start and a length
- syntax is `&s[0..2]`
- since a slice is a reference, same borrowing and dangling ref rules apply
- **string literals** are `&str` - slice to point in binary, immutable refs
- taking a string slice as param = can take `&String` or `&str`, since `&String` is implicitly deref coerced
- can also slice an array with same syntax