# 2. Idiomatic Rust Examples

### Usage of `iter`, `map`, `collect` together

```rust
fn main() {
    let numbers = vec![1, 2, 3, 4, 5];

    let doubled_numbers: Vec<i32> = numbers
        .iter()
        .map(|number| number * 2)
        .collect();

    println!("{:?}", numbers);
    println!("{:?}", doubled_numbers);
}
```

`iter()`: creates an iterator that allows you to traverse each element of a collection, such as a vector (`Vec`). The iterator in the example yields references (&i32) to each element one by one.

`map()`: takes each element from the iterator and applies a function (closure) to transform each element into something else.

`collect()`: gathers the transformed values from the iterator and converts them back into a collection (such as `Vec`).

### Usage of `zip` and `sum` in chain

```rust
fn main() {
    let vec1 = vec![1, 2, 3, 4, 5];
    let vec2 = vec![10, 20, 30, 40, 50];

    let output = vec1
        .iter()
        .zip(&vec2)
        .map(|(input1, input2)| input1 * input2)
        .sum::<i32>();

    let max_value = (output).max(1000);
    let min_value = (output).min(1000);

    println!("Maximum value: {}", max_value);
    println!("Minimum value: {}", min_value);
}
```

`zip()`: pairs elements from two iterators, creating a new iterator of tuples. It stops once one of the iterators runs out of items.
After zip in our example: `[(1,10), (2,20), (3,30), (4,40), (5,50)]`

`sum()`: consumes the iterator and adds all its elements together, giving you a single numeric result. `sum::<i32>()` explicitly defines the type of the resulting sum.

> ℹ️ Info
>
> Quick note about `max()` and `min()`:
> max(1000) returns the higher of the two values (output vs. 1000). min(1000) returns the lower of the two values (output vs. 1000).

### Usage of `fold` in chain

```rust
fn main() {
    let numbers = vec![1, 2, 3, 4, 5, 6, 7, 8, 9];
    let odd_numbers = numbers.iter().fold(Vec::new(), |mut acc, number| {
        if number % 2 != 0 {
            acc.push(number);
        }
        acc
    });

    println!("{:?}", odd_numbers);

    let sum = numbers.iter().fold(0, |acc, number| acc + number);

    println!("Sum of the numbers: {}", sum);
}
```

`fold()`: allows you to accumulate a value while iterating.

1. Initial value: `Vec::new()` is the starting value for `acc`. Here it creates `acc: Vec<&i32>`
2. `acc` is the accumulator which caries the result so for
3. `number` is the current item of the iterator.
