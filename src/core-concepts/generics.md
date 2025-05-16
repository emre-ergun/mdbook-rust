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

fn serialize_payload<T>(payload: &T) -> String{
    "placeholder".to_owned()
}
```

## Monomorphization

- This means that compiler stamps out a different copy of the code of a generic function for each concrete type needed.
- For example, if the serialize_payload function is called for T=String and T=i32, the compiler will generate different copies of the function for each concrete type needed.
- This process of monomorphization also happens with structs, enums, and implementation blocks
