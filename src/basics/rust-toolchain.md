# 1.1 Rust Toolchain

## Core Tools

| Tool     | Description                                                                                                                   |
| -------- | ----------------------------------------------------------------------------------------------------------------------------- |
| `rustc`  | The **Rust compiler**. It compiles `.rs` source code into binary executables or libraries.                                    |
| `cargo`  | The **Rust package manager and build tool**. It handles project creation, building, dependency management, and running tests. |
| `rustup` | The **Rust toolchain installer and version manager**. It lets you install and manage multiple Rust versions and components.   |

## Project & Dependency Tools

| Tool         | Description                                                                                               |
| ------------ | --------------------------------------------------------------------------------------------------------- |
| `crates.io`  | The **official package registry** for Rust libraries. It hosts reusable packages called "crates".         |
| `Cargo.toml` | The **manifest file** for a Rust project. It lists metadata, dependencies, and configuration for `cargo`. |

## Testing & Formatting Tools

| Tool           | Description                                                                             |
| -------------- | --------------------------------------------------------------------------------------- |
| `cargo test`   | Runs unit tests or integration tests for your Rust project.                             |
| `cargo fmt`    | Automatically **formats your code** using Rust’s official style guide.                  |
| `cargo clippy` | A **linter** that provides suggestions to improve code style and catch common mistakes. |

## Documentation

| Tool        | Description                                                                      |
| ----------- | -------------------------------------------------------------------------------- |
| `rustdoc`   | Generates HTML **documentation** from your Rust code using doc comments (`///`). |
| `cargo doc` | Builds documentation for your project and its dependencies using `rustdoc`.      |

## Nightly & Advanced Tools

| Tool           | Description                                                                              |
| -------------- | ---------------------------------------------------------------------------------------- |
| `cargo bench`  | Runs **benchmark tests** to measure performance. Available on **nightly**.               |
| `cargo expand` | Expands macros to show generated code. Useful for debugging procedural macros.           |
| `miri`         | An **interpreter** for Rust programs to detect **undefined behavior**. Works on nightly. |

## Toolchain Channels

| Channel   | Description                                                                                        |
| --------- | -------------------------------------------------------------------------------------------------- |
| `stable`  | Most reliable, updated every 6 weeks.                                                              |
| `beta`    | Next release candidate for stable. Used for testing.                                               |
| `nightly` | Updated daily, includes experimental features and tools. Needed for advanced or unstable features. |
