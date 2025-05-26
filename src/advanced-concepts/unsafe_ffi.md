# 3.3 Unsafe Rust and FFI

## Unsafe Rust

- To write unsafe code in Rust we should use unsafe keyword
- We can do 5 specific things, which we can not do without unsafe keyword, in unsafe block.
  1. Dereference a raw pointer
  2. Call an unsafe function
  3. Implement an unsafe trait
  4. Access/Modify a mutable static variable
  5. Access fields of a union.

## FFI

- aka Foreign Function Interface.

```rust
#[link(name = "adder", kind="static")]
extern "C" {
    fn add(a: i32, b:i32) -> i32;
}

fn main() {
    let x:i32;
    unsafe {
        x = add(1, 2);
    }
    println!("sum: {x}");
}
```
