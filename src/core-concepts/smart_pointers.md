# 2.7 Smart Pointers

## Box Smart Pointer

- Use cases of box smart pointer: `Box::new(<object>);`
  - First one is to avoid copying large amount of data when transferring ownership.
  - Box smart pointer is in combination with trait objects.
  - When you have a type of unknown size, you want to use box smart pointer in a context where the exact size is required
  - Box smart pointer is similar to unique smart pointer in C++

```rust
let ui_components: Vec<Box<dyn UIComponent>> =
        vec![Box::new(button_a), button_b, Box::new(label_a)];
// button_b has already defined as Box smart pointer
```

## Rc Smart Pointer (Reference Counter)

It provides shared ownership.

- `Rc<object_name>`. When it is created the reference counter is 1. Then each time it is cloned as `Rc::clone(&variable)`, reference counter is increased by 1.
- Rc smart pointer is similar to shared smart pointer in C++
- It can only be used in single-threaded applications. For multi-threaded applications you must use the atomically reference counted smart pointer

## RefCell Smart Pointer

- Rc smart pointer only allows immutable shared ownership of a value. With RefCell we can borrow mutably.
- RefCell uses unsafe rust code to work around of borrowing rules at compile time.
- RefCell must use carefully because responsibility of ownership is on programmer.

### Custom Smart Pointer

- Two important traits implemented by smart pointers
  - `std::ops::Deref` immutable dereferencing operations
  - `std::ops::Drop` when a value goes out of scope you can run code also known as destructor.

```rust
use std::ops::Deref;

// Define a generic smart pointer
struct MyBox<T>(T);

// Implement new method
impl<T> MyBox<T> {
    fn new(x: T) -> MyBox<T> {
        MyBox(x)
    }
}

// Implement Deref to allow `*` operator
impl<T> Deref for MyBox<T> {
    type Target = T;

    fn deref(&self) -> &Self::Target {
        &self.0
    }
}

// Implement Drop for custom cleanup
impl<T> Drop for MyBox<T> {
    fn drop(&mut self) {
        println!("Dropping MyBox with value!");
    }
}

// Usage example
fn main() {
    let x = 5;
    let y = MyBox::new(x);

    println!("x = {}", x);
    println!("*y = {}", *y); // works because of Deref
}
```
