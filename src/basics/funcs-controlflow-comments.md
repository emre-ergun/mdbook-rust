# 1.2 Functions, Control Flow, Comments

ℹ️ **Rust coding rules suggestions:**

| Item Type         | Case Style                   | Example                      |
| ----------------- | ---------------------------- | ---------------------------- |
| **Project Name**  | `snake_case` or `kebab-case` | `my_project` or `my-project` |
| **Variables**     | `snake_case`                 | `my_variable`                |
| **Functions**     | `snake_case`                 | `calculate_sum()`            |
| **Structs**       | `PascalCase`                 | `MyStruct`                   |
| **Enums**         | `PascalCase`                 | `MyEnum`                     |
| **Enum Variants** | `PascalCase`                 | `SomeVariant`                |
| **Constants**     | `SCREAMING_SNAKE_CASE`       | `MAX_LIMIT`                  |
| **Statics**       | `SCREAMING_SNAKE_CASE`       | `MAX_LIMIT`                  |
| **Modules**       | `snake_case`                 | `my_module`                  |
| **Crates**        | `snake_case`                 | `my_crate`                   |
| **Traits**        | `PascalCase`                 | `Display`                    |
| **Type Aliases**  | `PascalCase`                 | `MyType`                     |
| **Lifetimes**     | `lowercase with` `'`         | `'a`, `'static`              |

## Functions

Function definition syntax of Rust

```rust
fn function_name(parameter1: Type1, parameter2: Type2, ...) -> ReturnType {
    // function body
}
```

- The return type comes after the arrow ->.
- If the last expression in the function block is returned implicitly, you omit the semicolon.
- If you use return, you must end the line with a semicolon.

### When a generic type has to be used in the function definition

1. Basic generic function:

   ```rust
   fn identity<T>(value: T) -> T {
       value
   }
   ```

2. Generic with constraits (trait bounds)

   ```rust
   fn print_value<T: std::fmt::Display>(value: T) {
       println!("{}", value);
   }
   ```

3. Multiple generic parameters

   ```rust
    fn pair<T, U>(a: T, b: U) {
       // do something
   }
   ```

4. With return type and trait bounds

   ```rust
   fn add<T: std::ops::Add<Output = T>>(a: T, b: T) -> T {
       a + b
   }
   ```

5. Syntax with "where" keyword

   ```rust
   fn function_name<T, U>(param1: T, param2: U) -> ReturnType
   where
       T: Trait1 + Trait2,
       U: Trait3,
   {
       // function body
   }
   ```

## Control Flows

### Conditional statemens

| Syntax                    | Description                             |
| ------------------------- | --------------------------------------- |
| `if` / `else if` / `else` | Branches execution based on a condition |
| `match`                   | Pattern matching control flow           |

```rust
fn main() {
    if x > 0 {
        println!("Positive");
    } else if x < 0 {
        println!("Negative");
    } else {
        println!("Zero");
    }

    match number {
        1 => println!("One"),
        2 | 3 => println!("Two or Three"),
        _ => println!("Something else"),
    }
}
```

### Loops

| Syntax  | Description                       |
| ------- | --------------------------------- |
| `loop`  | Infinite loop (exit with `break`) |
| `while` | Loop while a condition is true    |
| `for`   | Loop over an iterator or range    |

```rust
fn main() {
    loop {
        if some_condition {
            break;
        }
    }

    while i < 10 {
        i += 1;
    }

    for x in 0..5 {
        println!("{}", x);
    }
}
```

### Loop control

| Syntax            | Description                             |
| ----------------- | --------------------------------------- |
| `break`           | Exit the loop                           |
| `continue`        | Skip the rest of current loop iteration |
| `break 'label`    | Break out of a named loop               |
| `'label: loop {}` | Label a loop to break from nested loops |

```rust
fn main() {
    'outer: for i in 0..3 {
        for j in 0..3 {
            if i == j {
                break 'outer;
            }
        }
    }
}
```

## Comments

| Comment Type         | Syntax      | Purpose                            |
| -------------------- | ----------- | ---------------------------------- |
| Line Comment         | `//`        | For quick notes or disabling lines |
| Block Comment        | `/* ... */` | Multi-line, can be nested          |
| Doc Comment (item)   | `///`       | Public API docs for items          |
| Doc Comment (module) | `//!`       | Docs for the current file/module   |

### Examples for Doc comments

1. Documenting a function

   ````rust
   /// Adds two numbers and returns the result.
   ///
   /// # Arguments
   ///
   /// * `a` - The first integer
   /// * `b` - The second integer
   ///
   /// # Example
   ///
   /// ```
   /// let sum = add(2, 3);
   /// assert_eq!(sum, 5);
   /// ```
   fn add(a: i32, b: i32) -> i32 {
       a + b
   }
   ````

2. Documenting a struct

   ```rust
   /// A simple structure to hold a point in 2D space.
   struct Point {
       /// The x-coordinate.
       x: f64,
       /// The y-coordinate.
       y: f64,
   }
   ```

3. Crate level documentation (usually in lib.rs)

   ```rust
   //! # My Math Crate
   //!
   //! This crate provides basic math utilities.
   //!
   //! ## Features
   //! - Addition
   //! - Subtraction

   /// Adds two numbers.
   pub fn add(a: i32, b: i32) -> i32 {
       a + b
   }
   ```

4. Module level documentation (usually in mod.rs)

   ```rust
   //! This module handles geometry-related computations.

   /// Computes the area of a rectangle.
   pub fn area(width: f64, height: f64) -> f64 {
       width * height
   }
   ```

✅ **Notes:**

- Markdown works in both `///` and `//!`.
- Use triple backticks ` ``` ` for code blocks.
- Run `cargo doc --open` to generate and view documentation in your browser.
