# 2.6 Lifetimes

## Concrete Lifetime

Borrow checker tracks the lifetime of the values and checks to make sure that references are valid at compile time.

- A concrete life time is the time during which a value exists at a particular memory location.
- A life time is started when a variable is created or moved into a particular memory location, and ends when a value is dropped or moved out of a particular memory location.

## Generic Lifetime

- lifetime specifiers are also known as generic lifetime annotations are a way to describe the relationship between lifetime of references.

```rust
fn first_turn<'a>(p1: &'a str, p2: &'a str) -> &'a str
// life time of the return value is equal the lifetime of the argument
// which has shortest lifetime. life time of p1 or life time of p2
```

> ℹ️ Info
>
> Life time of return value must be tied to the life time input parameters in general. It is because if a function returns reference that reference has to point to something passed in.

- ‘static lifetime
  - string slices has a static lifetime. This means we can return string slices created in the function as a return value

```rust
fn first_turn2(_p1: &str, _p2: &str) -> &'static str {
    let s1: &'static str = "Lets Get Rusty";
    s1
}
```

### Struct and Life time Elision

Basically, when it is evident that we want certain lifetimes to be available, the compiler can do that for us automatically. It has some rules as we listed below:

- Rust compiler follows three lifetime elision rules:

  - Each parameter that is a reference gets its own lifetime parameter
  - If there is only one input lifetime parameter, that lifetime is assigned to all output lifetime parameters
  - If there are multiple input lifetime parameters, but one of them &self or &mut self, the lifetime of self is assigned to all output lifetime parameters

- When storing references in struct we must add a generic lifetime annotation.

```rust
// lifetime elision
fn take_and_return_content(content: &str) -> &str {
    content
}
```
