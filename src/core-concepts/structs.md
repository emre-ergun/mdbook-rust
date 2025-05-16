# 2.3 Structs

## Structs and Implementation Block

Structs allow us group related data together.

```rust
#[derive(Debug)]
struct Product {
    name: String,
    price: f32,
    in_stock: bool,
}

impl Product {
    // Associated function
    fn new(name: String, price: f32, in_stock: bool) -> Self {
        Product {
            name,
            price,
            in_stock,
        }
    }

    // Associated function
    fn get_tax_rate() -> f32 {
        0.1
    }

    // immutable borrow of self
    fn calculate_sales_taxes(&self) -> f32 {
        self.price * Product::get_tax_rate()
//                   ^^^^^^^^^^^^^^^^^^^^^^
//                   Usage of the associated function
    }

    // mutable borrow of self
    fn set_price(&mut self, val: f32) {
        self.price = val;
    }

    // owned form of self
    fn buy(self) -> i32 {
        let name = self.name;
        println!("{name} was bought");
        123
    }
}

fn main() {
//  must be set as mutable because of the set_price()
    let mut phone = Product::new(String::from("Iphone"), 999.99, true);
//                  ^^^^^^^^^^^^^^^^^
//                  Most common usage of the associated function
    let sales_tax = phone.calculate_sales_taxes();
    println!("Sales tax: {sales_tax}");

    phone.set_price(100.00);
    println!("Sales tax: {}", phone.calculate_sales_taxes());

    println!("Invoice number: {}", phone.buy());
//                                  ^^^ The value of phone is moved
    phone.set_price(200.00);
//  ^^^^^^ error: the value of phone is dropped
}
```

Here in the example above we can see some definitions:

1. Immutable borrowing of self: It borrows self as immutable
2. Mutable borrowing of self: It borrows self as mutable. So, we can change the value of the attributes.
3. Self owned: It moves ownership to the member function.
4. Associated function: It is the function does not use the self as an argument but it is still the member function. Double column is used to call the function.

### Tuple Structs

The maximum number of elements allowed in a tuple or tuple struct is 12 for most standard trait implementations like Debug, Clone, PartialEq, etc., in the standard library.

```rust
fn main() {
    struct RBG(i32, i32, i32);
    struct CMYK(i32, i32, i32, i32);

    let color1 = RGB(255, 255, 255);
    let color2 = CMYK(0, 58, 100, 0);

    //unit-like struct
    struct MyStruct;
}
```

### Enums and Matching

```rust
enum ProductCategory {
    Books,
    Clothing,
    Electrics
}

enum Command {
    Undo,
    Redo,
    AddText(String),
    MoveCursor(i32, i32),
    Replace{
        from: String,
        to: String
    }
}

impl Command {
    fn serialize(&self) -> String {
        match self {
            Command::Undo => String::from(
                "{\"cmd\": \"undo\"}"
            ),
            Command::Redo => String::from(
                "{\"cmd\": \"undo\"}"
            ),
            Command::AddText(s) => format!(
                "{{\
                   \"cmd\": \"add_test\",\
                   \"text\": \"{s}\" \
                }}"
            ),
            Command::MoveCursor(x, y) => format!(
                "{{\
                   \"cmd\": \"move_cursor\",\
                   \"line\": {x} \
                   \"column\": {y} \
                }}"
            ),
            Command::Replace(from, to) => format!(
                "{{\
                   \"cmd\": \"replace\",\
                   \"from\": \"{from}\" \
                   \"to\": \"{to}\" \
                }}"
            ),
        }
    }
}

fn main() {
    let category = ProductCategory::Books;
}
```

Mathc Expression:

1. It should cover all branches.
2. `_` can be used to cover rest of branches.
3. For enums in Rust, each attribute can have different types.

## Special Enums in Rust

### Option

```rust
enum Option<T> {
    Some(t),
    None
}

fn main() {
    // if-let syntax is Rust-based feature
    // and it is useful pattern if you dont want to
    // implement None branch with `match` pattern
    if let Some(value) = username {
        println!("username: {value}");
    }
}
```

### Result

> ℹ️ Info
>
> `ok()` method of Result enum converts Result to Option

```rust
enum Result<E, T> {
    Err(E),
    Ok(T)
}
```
