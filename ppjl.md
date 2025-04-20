#rust

# Prgoramming rust : Ownership and borrow

## Ownership

The owning object gets to decide when to free the owned objecte: when the owner is destroyed its possession along with it.

In Rust, ownership is a core concept that ensures memory safety without a garbage collector. It’s a set of rules that govern how memory is managed, enforced at compile time. Here’s a concise explanation:

## Key Principles of Ownership

- Each value has a single owner: Every value in Rust is owned by exactly one variable or binding. When the owner goes out of scope, the value is automatically deallocated (no manual free needed).

- Only one mutable reference or multiple immutable references: At any given time, you can have either:
  - One mutable reference (&mut) to a value, allowing changes.
  - Any number of immutable references (&), allowing read-only access.
  - You cannot have mutable and immutable references simultaneously to prevent data races.

Ownership can be transferred: Ownership of a value can be moved to another binding (e.g., by assignment or passing to a function). This is called a **move**, and the original binding becomes invalid.

```rust
  let s1 = String::from("hello"); // s1 owns the String
  let s2 = s1;                   // Ownership moves to s2; s1 is no longer valid
  // println!("{}", s1);         // Error: s1 was moved
  println!("{}", s2);            // Works: s2 now owns the String
```

Rust extends this simple idea in several ways:

- You can move values from one owner to another. This allows you to build, rearrange , and tear down the trees
- Very simple types like integers, floating point numbers, and characters are excused from the ownership rules. These are called **Copy types**.
- The standard library provides the refernce-counted pointer types Rc and Arc, which allow values to have mulitple owners, under some restrictions
- You can "borrow a reference" to a value; references are non-owing pointers with limited Lifetimes

## Move

Move is tranfer of ownership to another binding

### Rc and Arc: Shared Ownership

```rust
let s = Rc::new("hello".to_string());
let t = s.clone();
let u = s.clone();
```

## Borrowing

To avoid moving ownership, you can borrow a value using references:

- Immutable borrow: let r = &s2; allows reading s2 without taking ownership.
- Mutable borrow: let r = &mut s2; allows modifying s2, but only one mutable borrow is allowed at a time.

## References
