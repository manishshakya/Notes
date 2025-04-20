#rust

# Programming Rust - Fundamental Types

## Fixed-Width Numeric Types

- unsigned types : u8, u16, u32, u64 and u128
- signed types : i8, i16, i32, i64 and i128
- floating point: f32 and f36
- usize and isize : theri percision matches the size of the address space of the target machine: they are 32 bits long on 32 bit archietectures and 64 bits long on 64 bit archietectures

## The bool Types

- it has two values true and false
- comparison operators like == and < produce bool results

## Characters

- `char` represents a single unicode character, as a 32-bit value
- Rust uses the `char` type for single characters in isolation, but uses the UTF-8 encode for strings and streams of text. So, a String represetns it text as a sequence of UTF-8 bytes, not as a array of characters
- Unicode defines the character set; UTF-8 is one way to encode those characters into bytes.
- As with byte literals, backslash escapes are required for a few characters
- single quote ('\''), backslash ('\\'), newline ('\n'), carriage return ('\r') and tab ('\t')
- you can wire ASCII character as '\xHH'
- you can write unicode character as '\u{HHHHHH}'

## Tuples

- A tupe is a pair, or triple, quadruple etc (hence n-tuple or tuple) of values of assorted types.
- examples : `let t = ("Troy",48085)`
- access: `t.0 or t.1`
- zero-tuple (): this is traditionally call the unit type beacuse it has one value ,als written ()

## Pointer Types

- Rust has serveral types that represents memory addresses.

### References

- A value of type &String (produnced "ref String") is a reference to a String value
- The expression &x produces a reference to x
- in Rust terminlology, we say that it borrows a reference to x
- Given a reference r, the expression \*r refers to the value r points to
- And like a C pointer, a reference does not automatically free any resources when it goes out of scope
- Rust references comes in two flavors:
  - &T : An immutable, shared reference. You can havve many shared references to a given value at atime but they are read0only: modifying the value they point to is forbidden
  - &mut T : A mutable, exclusive reference. You can read and modify the value it points to. But for as long as the reference exists, you may not have na other references of any kind to the value

### Boxes

- The simplest way to allocate a value in the heap is to use Box::new;

```rust
let t = (12,"eggs");
let b =Box::new();
```

### Raw Pointers

- Rust also has the raw pointer type *mut T and *const T.
- using a raw pointer is unsafe, because Rust makes no effort to track what it points to.
- However, you may only derefernce raw pointer within an unsafe block.
- An unsafte block is Rust's opt-in mechanism for advanced langugae features whose safety is up to you.

## Arrays, Vectors, and Slices

### Arrays

- The type [T; N] represents an array of N values, each of type T.
- An array's size is a constant determined at compile time and is part of the types; you can't append new elements or shrink an array

```rust
let numbers: [u32;6] = [1,2,3,4,5,6];
let companies =["ti","st","nxp"];
```

### Vectors

- the type Vec<T>, called a vector of Ts, in a dynmically allocated, growable sequence of values of type T.

```rust
let mut v1 = vec![2,3,5,7];
let mut v2 = Vec::new();
v2.push(1);
v2.push(2);
v2.push(3);
```

- Vec<T> consist of three values: a pointer to the heap-allocated buffer; the number of elements that buffer has the capacity to store; the number it actually contains now
- if you know the number of elements a vector willneed in advance use `Vec::with_capacity`

### Slices

- The types &[T] and &mut [T], called a shared slice of Ts and mutable slice of Ts are references to a series of elements that are a part of some other value, like an array or vector
- You can think a slice as a pointer to its first element, together with a count of the number of elements youc can access starting at that point
- A reference to a slice is a fat pointer: a two-word value comprising a pointer to the slice's first element and te number of elemnet in the slice

```rust
let v = vec![1,2,3,4];
let sv:&Vec<u32> =&v;

print(&v[0..2]); // print the first two elements of v
print(&v[2..]); // print element of v starting with v[2]
print(&v[1..3]); // print  v[1] and V[2]
```

## String Types

### String Literals

- String literals are enclosed in double quotes

```rust
  let speech ="\"Ouch!\" said the well. \n";
```

- A string may span multple lines:

```rust
println("Hello
World"); // newline and white space are preserved
  println!("Hello \
World"); // newline and white space are not preserved
")
```

### raw strings

- A raw string is tagged wiht the lowercase letter r. No escapde sequence are recognized

```rust
  let path =r"C:\Program Files\Gorillas";
  let speech =r###""Ouch!" said the well."###; //if you want to include quotes(")
```

### Byte Strings

- A String literal with the b prefix is a byte string
- Sucah a string is a slice of u8 vlaues that is bytes - rather than Unicode text

```rust
  let path =b"Gorillas";
```

### Stings in memory

- Rust strings are sequence of unicode characters but they are not stored in memory as array. Instead the are stored using UTF-8, a variable-width encoding .

### String

- &str is very much like &[T]: a fat pointer to some data.
- String is analogousto Vec<T>

### Other String-Like Types

Rust's solution is to offer a few string-like types for different situtaions

- Stick to String and &str for Unicode test
- When working with filenames, use std::path::PathBuf and &Path instead
- When workind with binary data that isn't UTF-8 encoded at all, use Vec<u8> and &[u8]
- When working with enviorment varialbe name and command line argument in the native from presented by the operating system, Use OsString and &OsStr.
- When interoperating with C libraties that use null-terminated strings use std:::ffi::CString and &CStr

## Type Alias

- similar to typdef in C

```rust
type Bytes = Vec<u8>;
```
