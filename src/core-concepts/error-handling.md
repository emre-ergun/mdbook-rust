# 2.8 Error Handling

Error handling is complicated by its nature. It might have a few steps as follows:

- Defining
- Propagating
- Handling or Discarding
- Reporting
  - Developer & End User

## Recoverable Errors

- Represented by the `Result<T, E>` enum.
- Can be handled by the program gracefully.
- Common for operations like file I/O or network access

```rust
fn read_file() -> Result<String, std::io::Error> {
    std::fs::read_to_string("data.txt")
}
```

> ℹ️ Use
>
> **match**, **?**, or **.unwrap_or()** to handle them.

## Unrecoverable Errors

- Represented by the `panic!` macro
- Program stops execution immediately
- Used when a bug is detected or something truly unexpected happens.

```rust
fn divide(a: i32, b: i32) -> i32 {
    if b == 0 {
        panic!("division by zero");
    }
    a / b
}
```

> ℹ️ **Result to Option or Option to Result**
>
> [github/novafacing/CONVERSATIONS](https://gist.github.com/novafacing/6e087e5a62301a5b5c5a3fbd95263356)
>
> Resource for helpful conversations

### Idiomatic Errors in Rust

```rust
pub trait Error: Debug + Display {
    pub fn source(&self) -> Option<&(dyn Error + 'static)> {
        None
    }
}
```

Implementation Error trait on our custom error types achieves a few things:

- Semantically marks types as errors
- Standardizes
  - Checking the source of the error
  - User facing reporting
  - Developer facing reporting

> ℹ️ **Note**
>
> Error trait is defined in the standard library.
> Error handling infrastructure and third party libraries are built on top of this trait.
> So, it is important that your types implement this trait so that they work with the error handling ecosystem.

## Basic Error Handling (using thiserror and anyhow crates)

Here you can find a working example created to show how to use thiserror and anyhow crates to handle errors

- Project structure

```bash
my_example
├── Cargo.toml
├── config.yaml
└── src
    ├── lib.rs
    └── main.rs
```

- `Cargo.toml` file

```toml
[package]
name = "my_example"
version = "0.1.0"
edition = "2024"

[dependencies]
anyhow = "1.0"
thiserror = "2.0.12"
serde = { version = "1.0", features = ["derive"] }
config = "0.15.11"
```

- `lib.rs` file

```rust
use serde::Deserialize;
use thiserror::Error;

/// Our application wide configuration
#[derive(Debug, Deserialize)]
pub struct Config {
    pub db_url: String,
    pub max_connection: u32,
}

#[derive(Debug, Error)]
pub enum AppError {
    #[error("Failed to load configuration: {0}")]
    ConfigError(#[from] config::ConfigError),
    #[error("Failed to connect to Database: {0}")]
    DatabaseError(String),
}

pub fn load_config() -> Result<Config, AppError> {
    let config = config::Config::builder()
        .add_source(config::File::with_name("config.yaml"))
        .build()?
        .try_deserialize::<Config>()?;

    Ok(config)
}

pub fn connect_to_db(url: &str) -> Result<(), AppError> {
    if url.starts_with("postgres://") {
        Ok(())
    } else {
        Err(AppError::DatabaseError(format!(
            "Unsupported database url: {url}"
        )))
    }
}
```

- `main.rs` file

```rust
use anyhow::{Context, Result};
use my_example::{connect_to_db, load_config};

fn main() {
    if let Err(e) = run() {
        eprintln!("❌ Application error: {e}");

        for cause in e.chain().skip(1) {
            eprintln!("🔎 Caused by: {cause}")
        }
        std::process::exit(1);
    }
}

fn run() -> Result<()> {
    let config = load_config().context("Could not load config.yaml")?;
    println!("✅ Loaded config: {:#?}", config);

    connect_to_db(&config.db_url).context("Failed to connect to database")?;

    println!("✅ Connected to database!");

    Ok(())
}
```

**Notes for example:**

1. The config crate makes it very ergonomic to load and parse configuration files (like YAML, TOML, JSON, etc.)
2. `thiserror` crate is used at the library side to define custom error type
3. `anyhow` crate is used to handle errors ergonomically at binaries and application side.

Basically we can make a table for the usage of these two important crates:

| Layer          | Use         | Why                                          |
| -------------- | ----------- | -------------------------------------------- |
| **Library**    | `thiserror` | Define and expose structured error types     |
| **App/Binary** | `anyhow`    | Consume errors and handle/report them easily |
