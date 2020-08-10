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

> "Rust’s naming convention for constants is to use all uppercase with underscores between words, and underscores can be inserted in numeric literals to improve readability"

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
