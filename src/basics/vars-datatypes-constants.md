# 1.1 Varibles, Data Types, Constants

## Variables

```rust
// main.rs

fn main() {
    // creation
    let a = 5;
    //   ^ here type is annotated implicitly
    let b: i16 = 10;
    //    ^^^^ here type is explicitly annotated as i16.
    // if we try to assing 5.0 float point to b, it will
    // be an error because of type annotation.

    // mutability
    let c = 10; // variables are immutable as default in Rust
    let mut d = 20;
    // ^^^^ 'mut' keyword is used to make variable mutable.
    d = 120; // now we can change the value because of mutability

    // shadowing
    let e = 10;
    let e = 110; // the variable 'e' is shadowed.
    println!("{e}"); // 110 will be printed out to the terminal.
}
```

## Data Types

```rust
// main.rs

fn main() {
    // Scalar Data Types which store single data type

    // boolean
    let b1: bool = true;

    // unsigned integers
    let i1: u8 = 1;
    let i2: u16 = 1;
    let i3: u32 = 1;
    let i4: u64 = 1;
    let i5: u128 = 1;

    // signed integers
    let i6: i8 = 1;
    let i7: i16 = 1;
    let i9: i32 = 1;
    let i10: i64 = 1;
    let i11: i128 = 1;

    // floating point numbers
    let f1: f32 = 1.0;
    let f2: f64 = 1.0;

    // platform specific integers
    let p1: isize = 1; // represents pointer sized signed integer
    let p2: usize = 1; // represents pointer sized unsigned integer

    // characters, &str, and String
    let c1: char = 'c';
    let s1: &str = "Rust";
    let s2: String = String::from("Rust");


    // Compound Data Types which store multiple data types

    // arrays
    let a: [i32, 5] = [1, 2, 3, 4, 5]; // all members have the same type
    let a1 = a[4]; // reach the any element of array by "variable[<index>]"

    // tuples
    let t1: (i32, i32, i32) = (5, 10, 15);
    // each member can have different type
    let t2: (i32, f64, &str) = (5, 10.5, "rust");

    let s1 = t2.2; // reach the any element by "variable.<index>"
    let (i1: i32, f1: f64, s1: &str) = t2; // the tuple can also be destructured

    let t1 = (); // empty tuple is the special tuple typle called "unit type"


    // Type Aliasing
    type model = u16;
    let model_year: model = 1996;
}
```

## Constants

| Constant                                    | Static                             |
| ------------------------------------------- | ---------------------------------- |
| SCREAMIN_SNAKE_CASE                         | SCREAMIN_SNAKE_CASE                |
| Explicit type annotation                    | Explicit type annotation           |
| Can not be mutated                          | Can be marked as mutated           |
| Value of it will be inlined at compile time | It occupies location in the memory |

```rust
// main.rs
const MAX_LIMIT: u8 = 100; // inlined where it is used
//              ^^^^ explicit type annotation needed
static MIN_NUMBER: i32 = -5; // occupy location in the memory
//                ^^^^ explicit type annotation needed
//    ^^^^^^^^^^^ SCREAMING_SNAKE_CASE needed

fn main() {

}
```
