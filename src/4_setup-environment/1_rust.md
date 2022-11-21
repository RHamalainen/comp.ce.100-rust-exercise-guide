### Install Rust and the cross-compiler toolchain

1. Install Rust from [rustup.rs](https://rustup.rs/).
2. Install nightly toolchain using a terminal.
    - `rustup install nightly`
3. Install the cross-compiler.
    - `rustup target add armv7a-none-eabi`

### Install LLVM

Rust generally comes with a compiler included, but we need some additional tools to cross-compile on PYNQ-Z1.
LLVM might already be available on your system, but in case you get a message like "Unable to find libclang: "couldn\'t find any valid shared libraries matching: [\'clang.dll\', \'libclang.dll\']...", you will need to install an LLVM compiler on your system.
The best way to install depends on the platform.

On Windows, go to [https://releases.llvm.org/download.html](https://releases.llvm.org/download.html), and install e.g. the newest version of LLVM.
[This link](https://github.com/llvm/llvm-project/releases/download/llvmorg-11.0.0/LLVM-11.0.0-win64.exe) to the latest 64-bit executable on Windows may work as a shortcut for you.

On a Ubuntu system, you can install a compatible LLVM compiler via e.g. clang.
Run `sudo apt-get install clang`.
On Arch Linux `sudo pacman -S clang` may work too.
