# Rust book notes

Some notes I took while reading the [Rust book](https://doc.rust-lang.org/book).

## 1. Getting Started

### Installation

Install `rustup` toolchain

Linux:

```bash
curl --proto '=https' --tlsv1.2 https://sh.rustup.rs -sSf | sh
```

### Update

- Update latest version `rustup update`
- Version check `rustc --version`

### Documentation

`rustup doc`

### Basic program

```rust
// main.rs
fn main() {
    println!("Hello, world!");
}
```

> Rust is an **ahead-of-time** compiled: there is no need to install Rust to run the executable.

```bash
rustc main.rs #compilation step
./main #run step (.main.exe on windows)
Hello, world!
```

### Cargo

Rust's build system and package manager. (like npm but better)

- `cargo new <project_name>` create new project
- `cargo build` compile
- `cargo build --release` compile with optimizations
- `cargo run` compile and run
- `cargo run --release` compile and run with optimizations
- `cargo check` check if code compiles
- `cargo update` update crates (dependencies)
- `cargo doc --open` generate and open documentation <3 (including crates)
- `cargo clippy` linter for Rust (needs to be installed)
- `cargo audit` check for vulnerabilities (needs to be installed)

> Running cargo new in an existing Git repo will not generate new Git files.

## 2. Guessing game (exercise)

Crates used:

- `std::io:rand`
  - `rand::Rng` random number generation
- `std::cmp` compare two values
  - `cmp::Ordering` check if number >,=,< than

## 3. Common programming concepts

### Variables

Variables are immutable by default.

> Add `mut` to make the variable mutable

```rust
let mut x = 5;
```

### Constants

Can't use mut with constants. Constants are always immutable.

```rust
const MAX_POINTS: u32 = 100_000;
```

> Convention: use all uppercase with underscores between words, and underscores can be inserted in numeric literals to improve readability

### Shadowing

Variable shadowing is allowed in Rust.

```rust
let x = 5;
let x = x + 1;
```

> Note that the variable stays immutable, because it's a new variable

```rust
// compiles
let spaces = "   ";
let spaces = spaces.len();
```

```rust
// does not compile
let spaces = "   ";
spaces = spaces.len(); // mutated type
```

> Pattern: shadowing can be used for temporary mutability

```rust
let mut data = get_vec();
data.sort();
let data = data;
// Here data is immutable.
```

### Types

> When converting a type, we must add type annotation.

```rust
let guess: u32 = "42".parse().expect("Not a number!");
```

**Scalar types**

> A scalar type represents a single value

- Integers
  - signed: `i8 - i128` and `isize`
  - unsigned: `u8 - u128` and `usize`
  - default `i32` (fastest)
- Floats
  - `f32` and `f64`
  - default `f64` (more precision)
- Boolean
- Char

> The `isize` and `usize` types depend on the kind of computer your program is running on: 64 bits if you’re on a 64-bit architecture and 32 bits if you’re on a 32-bit architecture.

> With the `--release` flag a wrapper is added to prevent panic for integer overflow. For example with u8 256 becomes 0, 257 becomes 1 and so on. Results in unexpected values.

- Numeric operations (same as any other language) +, -, \*, /, %

**Compound types**

> Compound types can group multiple values into one type.

- Tuples
  - Elements can have different types
  - Fixed length

```rust
let tup: (i32, f64, u8) = (500, 6.4, 1);
let (x, y, z) = tup; // destructuring : x = 500
let first = tup.0; // index
```

- Array
  - All elements of same type
  - Fixed length (unlike other languages)
  - Uses stack instead of heap

```rust
let a: [i32; 5] = [1, 2, 3, 4, 5];
let a = [3; 5]; // = [3,3,3,3,3];
let first = a[0]; // index
```

### Functions

> Convention: use snake case to name a function.

**Parameters (arguments)**

> The type of each parameter must be declared.

```rust
fn main() {
    another_function(5, 6);
}

fn another_function(x: i32, y: i32) {
    println!("The value of x is: {}", x);
    println!("The value of y is: {}", y);
}
```

**Statements & expressions**

> Rust is a expression-based language!

Statements: instructions that perform some action and do not return a value.

```rust
let y = 6; // statement
let x = (let y = 6); // invalid let y = 6 does not return a value unlike other languages
```

Expressions: evaluate to a resulting value.

```rust
let x = 5;

let y = { // the expression
  let x = 3;
  x + 1 // returns 4
};
```

> Note that there is no semicolon at the end if we would add a semicolon we would turn it into a statement.

**Functions with return values**

- declare return type after the `->`.
- return value is the last expression.
- return early by using the `return` keyword.
- return type is `()` when nothing is returned.

```rust
fn main() {
    let x = plus_one(5);
    println!("The value of x is: {}", x); // 6
}

fn plus_one(x: i32) -> i32 {
    x + 1;
}
```

### Comments

- Regular comments
  - `// Line comments`
  - `/* Block comments */`
- Documentation comments
  - `/// Generate docs for the following item`
  - `//! Generate library docs for the enclosing item.`

> Markdown is supported in comments!

````rust
/// Adds one to the number given.
///
/// # Examples
///
/// ```
/// let arg = 5;
/// let answer = my_crate::add_one(arg);
///
/// assert_eq!(6, answer);
/// ```
pub fn add_one(x: i32) -> i32 {
    x + 1
}
````

### Control flow

**if/else**

- Same as other languages
- Condition must be a `bool` (Contrary to Js)
- `if` is an expression

> Note: because `if` is an expression we can use it in a `let` statement.

```rust
let condition = true;
let number = if condition { 5 } else { 6 };
let number = if condition { 5 } else { "six" }; // Won't compile because of incompatible value types.
```

**Loops**

**`loop`**

- Loop over block of code
- Use `break` to exit loop
- Add return value after `break`

```rust
fn main() {
    let mut counter = 0;

    let result = loop {
        counter += 1;

        if counter == 10 {
            break counter * 2; // return value
        }
    };
}
```

> Use case: retry an operation you know might fail until it succeeds.

**`while`**

- Same as in other languages
- Same as loop, but with break condition.

> Note: is technically the same as a `loop` with a combination of `if`, `else` and `break;` but much cleaner.

**`for`**

- Iterate through an `iterator`

```rust
fn main() {
    let a = [10, 20, 30, 40, 50];

    for element in a.iter() {
        println!("the value is: {}", element);
    }
}
```

> Note: An array is not iterable by default so we should call the slice method `.iter()`.

```rust
// count down
fn main() {
    // Range iterator (1..4)
    // rev() reverses the range
    for number in (1..4).rev() {
        println!("{}!", number);
    }
    println!("LIFTOFF!!!");
}
```

> Note: The Range `(start..end)` contains all values with `x >= start` and `x < end`. Is empty unless `start < end`.

## 4. Understanding ownership

> Unique feature of Rust.

> Ownership enables Rust to make memory safety guarantees without a garbage collector.

### What is ownership?

> **Stack**
>
> - Know, fixed size.
> - Faster than heap.
> - LIFO (last in, first out).

> **Heap**
>
> - Unknown size or size that might change.
> - Slower than stack.
> - Dynamic allocation.

> Pushing to the stack is faster than allocating on the heap because the allocator never has to search for a place to store new data; that location is always at the top of the stack.

**Rust ownership**:

- Track what parts of code use what data on the heap.
- Minimize duplicate data on the heap.
- Cleanup unused data on the heap.

**Ownership rules**:

- Each value in Rust has a variable that's called its **owner**.
- There can only be one owner at a time.
- When the owner goes out of scope, the value will be dropped.

**Variable scope**

Variables are block scoped just like in Js.

```rust
fn main() {
    { // s is not valid here, it’s not yet declared
        let s = "hello"; // s is valid from this point forward

        // do stuff with s
    } // this scope is now over, and s is no longer valid
}
```

**The `String` type**

All data types covered in chapter 3 are all stored on the stack and popped off the stack when their scope is over. To illustrate the rules of ownership we will need a more complex data type like `String` which is allocated on the heap.

```rust
let s = String::from("hello");
```

> Note: `String` is allocated on the heap and as such is able to store an amount of text that is unknown to us at compile time. For example to store user input.

```rust
// String can be mutable
let mut s = String::from("hello");

s.push_str(", world!"); // push_str() appends a literal to a String

println!("{}", s); // This will print `hello, world!`
```

**Memory and allocation**

With the `String` type, in order to support a mutable, growable piece of text, we need to allocate an amount of memory on the heap, unknown at compile time, to hold the contents. This means:

- The memory must be requested from the memory allocator at runtime.
  - This is done by us when calling `String::from`. The implementation requests the memory it needs. This is pretty much universal in programming languages.
- We need a way of returning this memory to the allocator when we’re done with our `String`.
  - In other languages this would be handled by the garbage collector who keeps track and cleans up memory that is not used anymore. This can be prone to errors.
  - In Rust the memory is automatically returned once the variable that owns it goes out of scope.

```rust
{
  let s = String::from("Hello"); // s is valid from this point

  // so stuff with s
} // this scope is over and s is no longer valid.
```

**Move**

Multiple variables can interact with the same data in different ways.

Simple types, know fixed size (stack):

```rust
let x = 5; // bind value 5 to x
let y = x; // bind copy of x to y
// now we have two variables with value 5 pushed on the stack.
```

Complex types, unknown variable size (heap):

```rust
let s1 = String::from("hello");
let s2 = s1;
```

This is the representation of variable `s1`

Stack:

| Name     | Value   |
| -------- | ------- |
| pointer  | -> heap |
| length   | 5       |
| capacity | 5       |

Heap:

| Index | Value |
| ----- | ----- |
| 0     | h     |
| 1     | e     |
| 2     | l     |
| 3     | l     |
| 4     | o     |

When we assign `s1` to `s2` we copy the stack data, meaning we copy the pointer, the length and capacity. We do not copy the data on the heap that the pointer refers to.

> Note: this is know as a move in Rust.

> Note: If we would copy the heap data as well this could be very expensive in terms of runtime performance if the data on the heap were large.

Now there is a problem when `s1` and `s2` go out of scope they would both try to free the same memory.

> Note: Freeing memory twice is know as a _double free_ error and can lead to memory corruption.

To solve this problem Rust considers `s1` to no longer be valid and therefore Rust doesn't need to free anything when `s1` goes out of scope.

```rust
let s1 = String::from("hello");
let s2 = s1;

println!("{}, world!", s1); // compile error: value s1 borrowed after move
// s1 is no longer valid. If we print s2 it would work
```

**Clone**

If we do want to copy the heap data of the `String` and not just the stack data we can use the common method `clone`.

```rust
let s1 = String::from("hello");
let s2 = s1.clone();

println!("s1 = {}, s2 = {}", s1, s2); // since we cloned s1 this will work.
```

We don't need to use clone dealing with simple types since they will be stored entirely on the stack so no need to clone them.

```rust
let x = 5;
let y = x;

println!("x = {}, y = {}", x, y); // this also work since its an integer
```

Rust annotates these simple types with the `Copy` trait. The other types will have the `Drop` trait. A type can only have one of these traits.

**Ownership and functions**

The semantics for passing a value to a function is similar to assigning a value to a variable.

```rust
fn main() {
    let s = String::from("hello");  // s comes into scope

    takes_ownership(s);             // s's value moves into the function...
                                    // ... and so is no longer valid here

    let x = 5;                      // x comes into scope

    makes_copy(x);                  // x would move into the function,
                                    // but i32 is Copy, so it’s okay to still use x afterward

} // Here, x goes out of scope, then s. But because s's value was moved, nothing special happens.

fn takes_ownership(some_string: String) { // some_string comes into scope
    println!("{}", some_string);
} // Here, some_string goes out of scope and `drop` is called. The backing memory is freed.

fn makes_copy(some_integer: i32) { // some_integer comes into scope
    println!("{}", some_integer);
} // Here, some_integer goes out of scope. Nothing special happens.
```

**Return Values and Scope**

You can return the ownership, but this can be quite tedious having to return the ownership every time that's why rust has `references`.

```rust
fn main() {
    let s1 = gives_ownership();         // gives_ownership moves its return value into s1

    let s2 = String::from("hello");     // s2 comes into scope

    let s3 = takes_and_gives_back(s2);  // s2 is moved into takes_and_gives_back, which also moves its return value into s3
} // Here, s3 goes out of scope and is dropped. s2 goes out of scope but was
  // moved, so nothing happens. s1 goes out of scope and is dropped.

fn gives_ownership() -> String {  // gives_ownership will move its return value into the function hat calls it.
    let some_string = String::from("hello"); // some_string comes into scope

    some_string // some_string is returned an moves out to the calling function
}

// takes_and_gives_back will take a String and return one
fn takes_and_gives_back(a_string: String) -> String { // a_string comes into scope
    a_string  // a_string is returned and moves out to the calling function
}

```

> Note: it is possible to return multiple values using a tuple.

```rust
fn main() {
    let s1 = String::from("hello");

    let (s2, len) = calculate_length(s1);

    println!("The length of '{}' is {}.", s2, len);
}

fn calculate_length(s: String) -> (String, usize) {
    let length = s.len(); // len() returns the length of a String

    (s, length)
}
```

## References and borrowing

```rust
// See previous example: we use borrowing instead of tuples
fn main() {
    let s1 = String::from("hello");

    let len = calculate_length(&s1); // pass reference of s1

    println!("The length of '{}' is {}.", s1, len);
}

fn calculate_length(s: &String) -> usize { // s is a reference to a string
    s.len()
} // s goes out of scope, but doesn't have ownership of what it refers to, nothing extra happens
```

**s (stack)**

| Name    | Value     |
| ------- | --------- |
| pointer | -> s1 ptr |

**s1 (stack)**

| Name     | Value   |
| -------- | ------- |
| pointer  | -> heap |
| len      | 5       |
| capacity | 5       |

**Heap**

| Index | Value |
| ----- | ----- |
| 0     | h     |
| 1     | e     |
| 2     | l     |
| 3     | l     |
| 4     | o     |

**Mutable references**

> Note: To make a reference mutable add `mut` => `&mut`

```rust
fn main() {
    let mut s = String::from("hello");

    change(&mut s);
}

fn change(some_string: &mut String) {
    some_string.push_str(", world");
}
```

> Note: `mutable references have one big restriction!`: you can not borrow a mutable reference more than once in a particular scope.

This measure is to prevent race conditions (very nice!):

- Two or more pointers access the same data at the same time.
- At least one of the pointers is being used to write to the data.
- There’s no mechanism being used to synchronize access to the data.

> Note: reference scope starts from where the reference was introduced to where it was last used.

> Note: `{}` can be used to create a new scope, allowing for multiple mutable references, just not simultaneous.

```rust
let mut s = String::from("hello");

    let r1 = &s; // no problem
    let r2 = &s; // no problem
    // let r3 = &mut s; // BIG PROBLEM (does not compile)

    // {
    //    let r3 = &mut s;
    // } // r3 goes out of scope => NO PROBLEM

    println!("{} and {}", r1, r2);
    // r1 and r2 are no longer used after this point

    let r3 = &mut s; // no problem
    println!("{}", r3);
```

**Dangling references**

> Note: a dangling pointer references a location in memory that may have been given to someone else, by freeing some memory while preserving a pointer to that memory.

TODO: lifetimes?

## The slice type

**String slices**

> Note: the type that signifies a string slice is written as `&str`

```rust
let s = String::from("hello world");
let hello: &str = &s[0..5];
let world: &str = &s[6..11];
```

**s (stack)**

| Name     | Value           |
| -------- | --------------- |
| pointer  | -> heap index 0 |
| len      | 11              |
| capacity | 11              |

**world (stack)**

| Name     | Value           |
| -------- | --------------- |
| pointer  | -> heap index 6 |
| len      | 5               |
| capacity | 5               |

**Heap**

| Index | Value |
| ----- | ----- |
| 0     | h     |
| 1     | e     |
| 2     | l     |
| 3     | l     |
| 4     | o     |
| 5     |       |
| 6     | W     |
| 7     | o     |
| 8     | r     |
| 9     | l     |
| 10    | d     |

> Note: string literals are string slices!

```rust
// Good practice!
fn first_word(s: &String) -> &str { // can be used only by String
fn first_word(s: &str) -> &str { // can ve used by both String and &str

// one can pass a String like so
let my_string = String::from("hello world");
let word = first_word(&my_string[..]);
```

**Range syntax:**

- `[start_index..end_index]` start is included, end is excluded
- `[start_index..=end_index]` start is included, end is included
- `[..2]` start index can be dropped if starting from first index
- `[0..]` same for the end index
- `[..]` takes the entire string

## 5 Using structs to structure related data

## Extra learning material

- [Doug Milford's Rust tutorial series](https://www.youtube.com/watch?list=PLLqEtX6ql2EyPAZ1M2_C0GgVd4A-_L4_5&v=Az3jBd4xdF4)
