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

**run program**

Rust is an **ahead-of-time** compiled: there is no need to install Rust to run the executable.

```bash
rustc main.rs #compilation step
./main #run step (.main.exe on windows)
Hello, world!
```

## Cargo

Rust's build system and package manager. (like npm but better)

- `cargo new <project_name>` create new project
- `cargo build` compile
- `cargo build --release` compile with optimizations
- `cargo run` compile and run
- `cargo check` check if code compiles

_Hint: running cargo new in an existing Git repo will not generate new Git files._

## 2. Guessing game (exercise)

- `cargo update` update crates (dependencies)
- `cargo doc --open` generate and open documentation <3 (including crates)

Crates used:

- `std::io:rand`
  - `rand::Rng` random number generation
- `std::cmp` compare two values
  - `cmp::Ordering` check if number >,=,< than

## 3. Common programming concepts
