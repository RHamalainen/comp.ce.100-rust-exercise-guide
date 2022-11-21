## Register access

Register access in Rust is more complicated than in C.
Check `rust-by-example` on following topics:

- [variable bindings](https://doc.rust-lang.org/rust-by-example/variable_bindings.html)
- [control flow](https://doc.rust-lang.org/rust-by-example/flow_control.html)
- [functions](https://doc.rust-lang.org/rust-by-example/fn.html)

### Register access with C

How to set a byte's bit high using C?
Declare a macro for known 32-bit memory address that casts the value to 32-bit pointer to an 8-bit value.

```c
#define ADDRESS (uint8_t*) 0x42010002
```

We can then declare a macro for a particular combination of bits to help manipulating the register value correctly.

```c
#define A_BIT 0b00000001
```

Now we can store a new value to the memory address by using dereference-operator `*` and using a bitwise `OR` to set the particular bit.

```c
*ADDRESS |= A_BIT;
```

### Register access with Rust

Let's do the same task as above but using Rust.
First we declare an 8-bit pointer to a 32-bit address as constant globals.

```rust
const ADDRESS: *mut u8 = 0x4201_0002 as *mut u8;
const A_BIT: u8 = 0b0000_0001;
```

Rust does not allow direct register access and the compiler is aggressive at optimizing the executable.
Thus we use following special functions for reading from and writing to memory addresses.

- [`core::ptr::read_volatile`](https://doc.rust-lang.org/core/ptr/fn.read_volatile.html)
- [`core::ptr::write_volatile`](https://doc.rust-lang.org/core/ptr/fn.write_volatile.html)

```rust
// Read value from a memory address.
let mut value: u8 = core::ptr::read_volatile(ADDRESS);

// Modify the value.
value |= A_BIT;

// Write value back to the memory address.
core::ptr::write_volatile(ADDRESS, value);
```

For your convenience, we provide a method called `mutate_ptr` to help you to do all of the above in one call.

```rust
fn some_function() {
    mutate_ptr(ADDRESS, |val| val |= A_BIT);
}
```

Now that you know how to do register access, you might be able to rewrite the `setup` function from C in Rust.
