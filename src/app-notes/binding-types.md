# Binding, Rebinding, and Types

In Rust, creating a binding simply means assigning a value to a name. This binding cannot be changed unless explicitly declared as mutable (`mut`).

```rust
fn main() {
    let x = 10;  // binds integer value 10 to identifier `x`
    println!("The value of x is {}", x);

    // x = 20; // This line would cause a compile error because x is immutable
}
```

You can allow a binding to be modified by using the keyword `mut`:

```rust
fn main() {
    let mut y = 5;  // mutable binding
    println!("Initial value of y is {}", y);

    y = 15; // changing the value is allowed now
    println!("Modified value of y is {}", y);
}
```

| Type              | Keyword or syntax       | Usage Example           |
| ----------------- | ----------------------- | ----------------------- |
| Immutable binding | `let x = 5;`            | Can't change later      |
| Mutable binding   | `let mut x = 5;`        | Changeable              |
| Pattern binding   | `let (a, b) = (1, 2);`  | Destructuring tuples    |
| Reference binding | `let r = &val;`         | Borrowing references    |
| Shadowing binding | `let x = 1; let x = 2;` | Redefine existing names |

When defining a function, each parameter has a binding and type. Here `inputs` is a binding, `Vec<i32>` is a type.

```rust
fn test_function(mut inputs: Vec<i32>) {
    // do something
}
fn main() {
    let inputs = vec![1,2,3,4];
    // ^ no mut needed here
    test_function(inputs);
    //            ^ works fine
}

```

The reason is that `mut` we've just introduced appears in so-colled binding position.

```rust
fn foo_1(items: &[f32]) {
    //   ^^^^^  ------
    //  binding  type
    // (immut.) (immut.)
}

fn foo_2(mut items: &[f32]) {
    //   ^^^^^^^^^  ------
    //    binding    type
    //   (mutable) (immut.)
}

fn foo_3(items: &mut [f32]) {
    //   ^^^^^  ----------
    //  binding    type
    // (immut.)  (mutable)
}

fn foo_4(mut items: &mut [f32]) {
    //   ^^^^^^^^^  ----------
    //    binding      type
    //   (mutable)   (mutable)
}

struct Person {
    name: String,
    eyeball_radius: usize,
}

fn decompose(Person { name, mut eyeball_radius }: Person) {
    //       ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^  ------
    //                     binding                 type
    // (partially immutable, partially mutable) (immutable)
}
```

... and bindings, contrary to types, are local to the function:

```rust
fn foo(items: &mut Vec<usize>) {
    // When a type is mutable, you can modify the thing being
    // referenced:
    items.push(1234);

    // But if the binding remains immutable, you cannot modify
    // *which* thing is referenced:
    items = some_another_vector;
    //    ^ error: cannot assign to immutable argument
}

fn bar(mut items: &Vec<usize>) {
    // On the other hand, when a binding is mutable, you can change
    // *which* thing is referenced:
    items = some_another_vector;

    // But if the type remains immutable, you cannot modify the
    // thing itself:
    items.push(1234);
    //   ^^^^^ error: cannot borrow `*items` as mutable, as it is
    //         behind a `&` reference
}
```
