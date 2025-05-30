# 2.2 Borrowing

The act of creating a reference.

- References are pointers with rules/restrictions
- References do not take ownership.

Why borrow?

- Performance
- When ownership is not needed/desired.

## Rules

1. At any given time, you can have either one mutable reference or any number of immutable reference.
2. References must always be valid.

## Which problems borrowing solves

1. Data races.
2. Dangling references.

## Examples

### 1. Referenced Argument

```rust
fn main() {
    let s1 = String::from("Hello Rust!");

    print_string(&s1);
//               ^^^ Instead of moving ownership
//                   Reference of the value is used
    println!("s1: {s1}")
//                ^^^ No error: the value is not moved
}

fn print_string(p1: &String) {
    println!("String in function: {p1}");
}
```

### 2. Mutable Reference

Here in the example mutable and immutable references are used together. Normally it violates the first borrowing rules. But Rust borrow checker is smart enough to understand that immutable reference is used before the mutable reference. This is a feature in rust called non-lexical lifetimes.

```rust
fn main() {
    let mut s1 = String::from("Hello Rust");
//     ^^^ mut keyword is used because mutable reference is needed.
    let r1 = &s1;
    print_string(r1);
    let r2 = &mut s1;
    add_string(r2);
    println!("s1: {s1}");
}

fn add_string(p1: &mut String) {
    p1.push_str(" World");
//  ^^^ here we can call p1 reference directly.
//  Because it is automatically dereferenced.
//  Otherwise (*p1).push_str() would be used.
}

fn print_string(p1: &String) {
    println!("String in function: {p1}");
}
```

## Borrow Checker

The borrow checker is part of Rust's compiler that ensures that references are always valid. It is tasked with enforcing of a number of properties:

- All variables must be initialized before used.
- You can't move same value twice.
- You can't move a value while it is borrowed.

So it checks to see that there is no ambiguity in the code that would point to an invalid reference in memory.
