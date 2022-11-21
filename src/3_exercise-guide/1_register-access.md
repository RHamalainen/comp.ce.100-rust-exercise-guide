## Register access

Register access in Rust is more complicated than in C. 
Please refer to the Rust book for the following relevant concepts that will not be explained here: [variable bindings](https://doc.rust-lang.org/rust-by-example/variable_bindings.html), [mutability](https://doc.rust-lang.org/rust-by-example/variable_bindings/mut.html), [constants](https://doc.rust-lang.org/rust-by-example/custom_types/constants.html?highlight=const#constants), [for-loops](https://doc.rust-lang.org/std/keyword.for.html).

### C

In C, you might declare a macro with an 8-character hexadecimal, representing a 32-bit memory address, that is then cast into a 32-bit pointer to an 8-bit value, like so:

```c
#define ADDRESS (uint8_t*) 0x42010002
```

Then you'd perhaps define a particular combination of bits to help setting a particular bit in the 8-bit register value:

```c
#define A_BIT 0b00000001
```

And finally, you might set the value in the register by first looking up the pointer with `*`, and then using a bitwise OR to set the particular bit:

```c
*ADDRESS |= A_BIT;
```

### Rust

In Rust, you'd first declare the 32-bit address to an 8-bit value and the bit combination as constant globals like so:

```rs
const ADDRESS: *mut u8 = 0x42010002 as *mut u8;
const A_BIT: u8 = 0b00000001;
```

Now's the complicated part.
Rust doesn't allow direct register access, and the compiler is more aggressive at optimizing.
That's why we must use a set of methods designed for register access, called `core::ptr::read_volatile` and `core::ptr::write_volatile`.

First, we must retrieve the 32-bit pointer to the 8-bit value into a **mutable binding**:

```rs
let value: *mut u8 = core::ptr::read_volatile( ADDRESS );
```

Then we must flip the bit, and write back into the register at the end of the 32-bit pointer:

```rs
value |= A_BIT;

// Write `value` back to main memory at `ADDRESS`
core::ptr::write_volatile( ADDRESS, value );
```

For your convenience, we have also provided a method called `mutate_ptr` to help do all of the above in one go:

```rs
const ADDRESS: *mut u8 = 0x42010002 as *mut u8;
const A_BIT: u8 = 0b00000001;

fn some_function() {
    mutate_ptr(ADDRESS, |val| val |= A_BIT);
}
```

Now that you know how to do register access, you might be able to rewrite the `setup` function from C in Rust.
