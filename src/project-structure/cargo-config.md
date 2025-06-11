# .cargo/config.toml

The `.cargo` folder in Rust project is optional but important for custom configuration and overrides. It plays different role than `Cargo.toml` or `src/` but is still usefull for more advanced Rust setups.

## `config.toml`

This is the most command file in `.cargo` folder. You can use it to:

### 1. Set a custom build target

Useful for cross-compiling

```toml
[build]
target = "thumbv7em-none-eabihf"
```

### 2. Specify a custom linker

For embedded or non-standard targets

```toml
[target.thumbv7em-none-eabihf]
linker = "arm-none-eabi-gcc"
```

### 3. Change target directory

Keep build artifacts out of the `target/` folder.

```toml
[build]
target-dir = "build"
```

### 4. Enable or override build scripts

Force features, set environment variables, etc.

```toml
[env]
RUSTFLAGS = "-C target-cpu=native"
```

### Example: Embedded Rust Project

```toml
[build]
target = "thumbv7em-none-eabihf"

[target.thumbv7em-none-eabihf]
runner = "probe-rs run"
linker = "arm-none-eabi-ld"
rustflags = [
  "-C", "link-arg=-Tlink.x"
]
```
