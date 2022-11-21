## Special integer types

Integer type `u32` is always 32-bit unsigned integer and `i64` is always 64-bit signed integer.
Special integer types `usize` and `isize` vary on size between platforms.
The size of `usize` is how many bytes it takes to reference any memory address.
For example in PYNQ-Z1 this requires 32-bit integer since it's memory address width is 32 bits.
On your own computer it might be 64 bits.
Special type `isize` is used as signed difference between two addresses.

Rust requires you to use `usize` for accessing an array.

```rust
let my_array: [u8; 3] = [5, 6, 7];
let my_value = my_array[2];
```

In above example, the type of value `2` is `usize`.

You can explicitely cast an integer to pointer-width type using `as`-operator.

```rust
let my_index: u32 = 2;
let my_value = my_array[my_index as usize];
```
