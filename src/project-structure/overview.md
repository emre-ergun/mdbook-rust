# Overview

## Basic Components

### 1. Packages

They contain one or more crates that provide a set of functionality. Packages allow you to build, test, and share crates.

- **Cargo.toml:** It describes the package and defines how to build crates.
- **Rules**
  - Must have at least one crate
  - At most one library crate
  - Any number of binary crates

**An example for package** : Here in the example `bin` folder is used to specify binary crates.

```bash
my_package
├── Cargo.lock
├── Cargo.toml
└── src
 ├── bin
 │   ├── another_one.rs
 │   └── main.rs
 └── lib.rs
```

Instead of using `bin` folder to specify binary crates, we can define them in the `Cargo.toml` file with the name and path as follows:

```toml
[package]
name = "my_package"
version = "0.1.0"
edition = "2024"

[[bin]]
name = "main"
path = "src/main.rs"

[[bin]]
name = "another_one"
path = "src/another_one.rs"

[dependencies]

```

With the file structure:

```bash
my_package
├── Cargo.lock
├── Cargo.toml
└── src
 ├── another_one.rs
 ├── lib.rs
 └── main.rs

```

> ℹ️ **Info**
>
> If we have more than one binary crate, to run one of the binaries we should call it with the name:
>
> `cargo run --bin <binary_name>`

### 2. Crates

A tree of modules that produce a library or execute.

### 3. Modules

- Organize code for readability and reuse
- Control scope and privacy
- Contain items (functions, structs, enums, traits, etc.)
- Explicitly defined (using the mod keyword)
  - Not mapped to the filesystem
  - Flexibility and straight forward conditional compilataion.

> ℹ️**Info**
>
> Rust modules are not mapped into filesystem. So, there is no way to tell a module, which is defined in `lib.rs` or another file, is a module that is submodule of the crate. That is why we should use `mod <module_name>` to use the module as submodule of a crate.
>
> See the example below:

```bash
├── Cargo.lock
├── Cargo.toml
└── src
    ├── a_submodule.rs
    └── lib.rs

```

```rust
//! src/a_submodule.rs

pub fn test_func() {
    println!("test function")
}
```

```rust
//! src/lib.rs

mod a_submodule;

use a_submodule::test_func;
//use a_submodule::*;

pub fn main_func() {
    test_func();
}
```
