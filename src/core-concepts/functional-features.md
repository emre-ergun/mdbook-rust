# 2.9 Functional Features

## Closures

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

## Iterator Pattern

- Standard Rust library has an iterator trait looks like this:

```rust
trait Iterator {
  type Item; // associated type instead of generic
  fn next(&mut self) -> Option<Self::Item>;
}
```

- The ‘next’ method has to be implemented by the structure that wants to use iterator feature.
- type Item: it is an Associated type. It is similar to generic except they are restricted to one concrete type per trait implementation.
- Another important trait in the standard library is IntoIterator trait:

```rust
trait IntoIterator {
  type Item;
  type IntoIter = Iterator;

  fn into_iter(self) -> Self::IntoIter;
}
```

- Usage iterator types - Iterator over Collections (hash map)

```rust
//scores is a hash map with type HashMap<String, i32>
for (key: String, value: i32) in scores // -> instead of scores.into_iter()
for (key: &String, value: &i32) in &scores // -> instead of scores.iter()
for (key: &String, value: &mut i32 in &mut scores // instead of scores.iter_mut()
```
