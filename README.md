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
- `cargo check` check if code compiles
- `cargo update` update crates (dependencies)
- `cargo doc --open` generate and open documentation <3 (including crates)

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
