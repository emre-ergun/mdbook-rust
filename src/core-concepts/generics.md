# 2.4 Generics

We use generics to create definitions for items like function signatures or structs, which we can then use with many different concrete data types.

```rust
struct BrowserCommand<T> {
    name: String,
    payload: T,
}

impl<T> BrowserCommand<T> {
    fn new(name: String, payload: T) -> Self {
        BrowserCommand { name, payload }
    }
}

/// Implement the Generic T for String
impl BrowserCommand<String> {
    fn print_payload(&self) {
        println!("payload: {}", self.payload);
    }
}
```

## Most common generic definitions in Rust

```rust
enum Option<T> {
    Some(T),
    None
}

enum Result<T, E> {
    Ok(T),
    Err(E)
}
```

## Monomorphization

- This means that compiler stamps out a different copy of the code of a generic function for each concrete type needed.
- For example, if the BrowserCommand struct is called for T=String and T=i32, the compiler will generate different copies of the struct for each concrete type needed.
- This process of monomorphization also happens with structs, enums, and implementation blocks
