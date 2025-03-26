# 3. TDD with Rust Libraries

You might've heard that it's useful to name tests based on their preconditions and expectations.

Usually that's true - if we were writing a shop, it could be useful to structure its tests like so:

```rust
#[cfg(test)]
mod tests {
    use super::*;

    mod cart {
        use super::*;

        mod when_user_adds_a_flower_to_their_cart {
            use super::*;

            #[test]
            fn user_can_see_this_flower_in_their_cart() {
                /* ... */
            }

            #[test]
            fn user_can_remove_this_flower_from_their_cart() {
                /* ... */
            }

            mod and_submits_order {
                /* ... */
            }

            mod and_abandons_cart {
                /* ... */
            }
        }
    }
}
```

> ℹ️ Info
>
> Creates which will be used only for testing purpose can be added under the `[dev-dependencies]` in the Cargo.toml file of the project. `[dev-dependencies]` are not allowed to be used in the library code(only with test configuration code)

```toml
[package]
name = "lib-a"
version = "0.1.0"
edition = "2021"

[dependencies]
rand = "0.8.5"

[dev-dependencies]
rand_chacha = "0.3.1"
approx = "0.5.1"
```

```rust
use rand::{Rng, RngCore};
use approx::assert_relative_eq;
//  ^^^^^^ use of undeclared crate or module "approx"

/*
* Library code here
*/

#[cfg(test)]
mod tests {
    use super::*;
    use approx::assert_relative_eq;
    use rand_chacha::ChaCha8Rng;
}
```
