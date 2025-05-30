# Workspace

## Config.toml

### Config.toml file for the workspace

```toml
[workspace]
# for Rust Edition 2021 and later, use 1 before the Rust Edition 2021
resolver = "2"

# members used to define creates will be used in the project
# members = ["lib-a", "lib-b"] is alternative usage
members = ["libs/*"]

# This section is optional
# If you would like to manage dependencies from the main Config.toml
# You can add them here as follow
[workspace.dependencies]
anyhow = "1.0"
serde = { version = "1.0", features = ["derive"] }
```

### Config.toml file for the specific create in the workspace

```toml
[package]
name = "lib-a"
version = "0.1.0"
edition = "2021"

[dependencies]
# If the crate is defined in the main Config.toml file
anyhow = { workspace = true }
rand = "0.8.5"

# dev-dependencies used for testing purpose
[dev-dependencies]
rand_chacha = "0.3.1"
```

### Config.toml file for library specialized for wasm

```toml
[package]
name = "lib-wasm"
version = "0.1.0"
edition = "2021"
description = "A wasm-library"
license = "MIT"
repository = "https://github.com/emre-ergun/mdbook-rust.git"

# library type has to be added for wasm build
# type of the library is C dynamic library
[lib]
crate-type = ["cdylib"]

[dependencies]
rand = "0.8.5"
getrandom = { version = "0.2.15", features = ["js"] }
wasm-bindgen = "0.2.100"
# a lib from the same workspace to use it another library
lib-a = { path = "../lib-a/" }
```
