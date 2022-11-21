### Install Rust and the cross-compiler toolchain

1. Install Rust from [rustup.rs](https://rustup.rs/).
2. Install nightly toolchain using a terminal.
    - `rustup install nightly`
3. Install the cross-compiler.
    - `rustup target add armv7a-none-eabi`

### Install LLVM

Rust toolchain contains a compiler, but in this project, cross-compiling to PYNQ-Z1 also requires `clang` which is an LLVM native C compiler.

If you get a message like "Unable to find libclang: "couldn\'t find any valid shared libraries matching: [\'clang.dll\', \'libclang.dll\']...", then your system does not contain `clang`.
Installing method depends on the platform.

- Windows
    - [Download and install LLVM](https://releases.llvm.org/download.html).
- Linux
    - Ubuntu
        - `sudo apt install clang`
