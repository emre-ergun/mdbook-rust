# 2.1 Ownership

Ownership is a strategy for managing memory(and other resources) through a set of rules checked at compile time.

## Rules

1. Each value in Rust has a variable that is called its owner.
2. There can only be one owner at a time.
3. When the owner goes out of scope, the value will be dropped.

## Which Problems Ownership solves?

1. Memory/Resource Leaks
2. Double free
3. Usage after free

## Examples

### 1. Scope

```rust
fn main() {
    {
        let s1 = String::from("Hello Rust!");
    }
//  ^ s1 is dropped, because it goes out of scope

    println!("s1 is: {s1}");
    //               ^^^ error: can not find value s1 in this scope
}
```

### 2. Ownership Moved

There can only be one owner at a time

```rust
fn main() {
    let s1 = String::from("Hello Rust!");
    let s2 = s1;
//  ^^^^^^^^^^^^ value of s1 is moved to s2
//  s1 is not owner of the value anymore
    println!("s1 is: {s1}");
//                   ^^^^ error: s1's value is moved,
//                        It can not be borrowed.
}
```

> ℹ️ INFO
>
> Ownership is not being moved for primitive types. In Rust, primitives that are entirely stored on the stack, such as integers, floating point numbers, booleans, or characters are cloned by default.
>
> These types are cheap to clone. So there is no material difference between cloning and moving the values.

### 3. Ownership Moved to an Argument of the Function

```rust
fn main() {
    let s1 = String::from("Hello Rust!");

    print_string(s1);
//              ^^^ ownership of the value moved here
    println!("s1 is: {s1}");
//                   ^^^ error: value is moved before
//                       It can not be borrowed.
}

fn print_string(p1: String) {
    println!("String in function: {p1}");
}
//^ p1 is dropped. It was the borrowed value of s1

```
