# 1.8 Closures

- Closures are anonymous functions which you can store in variables or pass as argument to other function.
- Closures can capture variables in the scope which they are defined
- `Fn`: Immutably borrow variables in environment
- `FnMut`: Mutably borrow variables in environment. can change environment.
- `FnOnce`: Take ownership of variables in environment. can only be called once.
  - move keyword can use before | |
  - in use case of using thread we can use move keyword for data protection.

## Function Pointers

- Fn used for closures. Instead, fn used for function pointers
- Function pointers are similar to closures **except** they don’t capture variables in their environment.

```rust
// function defined by closures (Fn())
fn are_both_true<T, U, V>(f1: T, f2: U, item: &V) -> bool
where T: Fn(&V) -> bool, U: Fn(&V) -> bool {
    f1(item) && f2(item)
}

// function defined by function pointers (fn())
fn are_both_true<V>(f1: fn(&V) -> bool, f2: fn(&V) -> bool, item: &V) -> bool {
    f1(item) && f2(item)
}
```

- If we want to use a variable in the closure environment we can not pass it to the function as fn (function pointer). We should use Fn(Closure call). Coercion does not work for this kind of situations.
- Closures can be coerced to function pointer if they are not capturing variable from the environment.
- Function pointers can also be coerced to closures.
