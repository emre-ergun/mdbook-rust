# 2.5 Traits

A trait defines the functionality a particular type has and can share with other types. We can use traits to define shared behavior in an abstract way. We can use trait bounds to specify that a generic type can be any type that has certain behavior.

> ℹ️ Info
>
> Rust does not support classical inheritance like other OO programming languages. Instead, it uses traits similar to interfaces in Java.

```rust
trait Park {
    fn park(&self);
}

trait Paint {
    // default implementation in trait
    fn paint(&self, color: String) {
        println!("Painting object: {}", color);
    }
}

struct VehicleInfo {
    make: String,
    model: String,
    year: u16,
}

struct Car {
    info: VehicleInfo,
}

impl Park for Car {
    fn park(&self) {
        println!("Parking car");
    }
}

// no need to implement the paint function because of default implementation
// in the trait
impl Paint for Car {}

struct Truck {
    info: VehicleInfo,
}

impl Truck {
    fn unload(&self) {
        println!("Unloading truck");
    }
}

impl Paint for Truck {
    fn paint(&self, color: String) {
        println!("Painting truck: {}", color);
    }
}

impl Park for Truck {
    fn park(&self) {
        println!("Parking truck");
    }
}

struct House {}

impl Paint for House {
    fn paint(&self, color: String) {
        println!("Painting house: {}", color);
    }
}
```

## Polymorphism

- **Polymorphism** is a concept in programming that allows objects to be treated as instances of their parent class rather than their actual class.
- This means that a single function can operate on objects of different classes.
- In Rust, polymorphism is achieved through traits and generics, which are powerful tools for writing flexible and reusable code.

## Trait Bounds

Three types of trait bound as it is seen below:

```rust
fn paint_red_zero<T>(object: &T)
where
    T: Paint + Park,
{
    object.paint("red".to_string());
}

fn paint_red_one<T: Paint>(object: &T) {
    object.paint("red".to_string());
}

fn paint_red_two(object: &impl Paint) {
    object.paint("red".to_string());
}
```

## Super Trait

Here is the paint is the super trait.

- Anything that implements Vehicle also implements Paint
- The functionality of the trait relies on the super trait.

```rust
trait Paint {
    fn paint(&self, color: String) {
        println!("Painting object: {}", color);
    }
}

trait Vehicle: Paint + AnotherTrait {
    fn park(&self);

    // Traits can have associated functions
    fn get_default_color() -> String {
        "black".to_owned()
    }
}
```

## Trait Objects

dyn stands for dynamic dispatch

```rust
fn create_paintable_object(vehicle: bool) -> Box<dyn Paint> {
    if vehicle {
        Box::new(Car {
            info: VehicleInfo {
                make: "Toyota".to_owned(),
                model: "Camry".to_owned(),
                year: 2018,
            },
        })
    } else {
        Box::new(House {})
    }
}

let paintable_objects: Vec<&dyn Paint> = vec![&car, &house];
```

### Static Dispatch

Where the compiler knows which concrete methods to call at compile time.

### Dynamic Dispatch

Where the compiler can not figure out which concrete methods to call at compile time. So, instead, it inserts a little bit of code to figure that out at runtime

- Advantage of dynamic dispatch is flexibility.
- it adds runtime performance cost

> ℹ️ Info
>
> **_To convert from Box smart pointer to reference we can use as_ref() method._**
