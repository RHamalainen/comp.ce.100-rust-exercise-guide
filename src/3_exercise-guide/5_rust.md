## Other things

### Printing

Like in the C-version, we must use a platform provided implementation of the `print`-functionality.

Several facilities for printing are provided by the course template.
The simplest way to print is through the `print64` and `println64` macros exported in `print.rs`:

```rust
println64!("Hello World from Rust!");

println64!("This is an integer: {}.", 42);
```

The `print64` and `println64` macros print a maximum of 64 characters per call using Rust style format. 
Alternatively, you could call `xil::xil_printf` of the C-FFI to use the Xilinx version of the standard `printf` C-interface and C format strings:

```rust
unsafe {
    xil::xil_printf("Hello World via xil_printf\0".as_ptr());

    xil::xil_printf("This is an integer: %d\0".as_ptr(), 42);
}
```

As shown above, using this interface requires you to use raw pointers, unsafe and nul-terminated strings like so `"Hello World!\0"`, but it gets rid of the 64 byte limitation.

### [Unsafe](https://doc.rust-lang.org/book/ch19-01-unsafe-rust.html)

If the compiler can not verify a fragment of the code, you have to wrap it inside an `unsafe`-block.

You have to use unsafe code to:

- dereference a raw pointer,
- call an unsafe function or method,
- access or modify a mutable static variable,
- implement an unsafe trait, or
- access fields of `union`s.

For example reading from or writing to an arbitrary physical memory address is unsafe.

```rust
let value = unsafe { read_volatile(address) };
```

Also calling a C-function via foreign function interface is unsafe.

```rust
unsafe {
    xil::Xil_ExceptionEnable();
}
```

The programmer is responsible for verifying the code inside unsafe block.
It might be the case that most runtime errors will occur inside of `unsafe {}`-blocks.

### Xilinx functions

---
You do not have to use Xilinx's board support functions in this course work.

---

The basic exercise template uses special functions from the Xilinx Cortex-A9 BSP.
These functions are accessed from Rust using the `xil-custom-sys` crate, which contains the pre-built Xilinx driver package static library with soft-floats (`libxil_sf.a`).
The `main.rs` of the template renames the library as `xil`, thus the Xilinx BSP functions are accessed as `xil::*`.
