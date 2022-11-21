# Building and running the application on-device

Clone the code to your local computer using terminal.

`git clone git@github.com:RHamalainen/alien-shooter-template-rs.git`

This downloads the project directory `alien-shooter-template-rs` to your computer.

The downloaded Rust project comprises board configuration files, source code and build configurations.

|Path|Description|Notes|
|---|---|---|
|`src/`|Rust source code|You have to edit these|
|`pynq/`|PYNQ-Z1 board configuration files||
|`Cargo.toml`|Rust library dependencies and package configuration||
|`.cargo/config`|Rust build (compile and link) configurations||

Normally building and running a Rust application is usually as simple as invoking the main toolchain build command.
Unfortunately, we're cross-compiling from desktop to another device provided by a manufacturer with specific requirements.
Because of this, we have to provide the location of the manufacturer's tools with the build command.

## Build the application

Switch to your project folder using terminal.

`cd alien-shooter-template-rs`

Install correct cross-compiler for this project.
PYNQ-Z1 contains dual-core ARM Cortex-A9, so the instruction set used is `armv7a`.
We also do not use an operating system.
We also use embedded application binary interface (EABI).
Thus the correct command is following.

`rustup target add armv7a-none-eabi`

We're going to need to provide the location of the manufacturer's tools as an environment variable. 
Scripts for doing this are provided.

Setting the environment variable is different on different types of terminals.

---
Using Windows command prompt is not recommended.

---

|Terminal|Command|
|---|---|
|Git Bash, WSL, native Linux|`source ./scripts/tc219.env`|
|PowerShell|`. ./scripts/tc219.ps1`

After the environment variables are set, we can execute `build`-command.

`cargo build`

This attempts to produce an executable file that we can run on PYNQ-Z1 ARM processors.

## Open an input-output interface to the board

To run the executable on the board, we have to program it to it.
Before running the application, we will want to open an interface to the board that can display what the board is saying for us.
In this exercise, we're going to open an `UART` over `USB` connection to the board.

First, we'll need to figure out which COM port the device maps itself to.

1. Solve which `COM`-port the PYNQ-Z1 is connected to.
    1. Open `Windows Device Manager`.
    2. Check what ports are listed under `Ports (COM & LPT)`.
    3. Turn on PYNQ-Z1.
    4. Check what `COM`-port appeared.
        - This port can vary.
        - It might be e.g. `COM5` or `COM6`.
2. Open `PuTTY`.
    1. Select the following settings.
        * `connection type: serial`
        * `serial line: COM ???`
            - Check step 1.
        * `speed: 115200`
    2. (Optional) You can save the configuration by typing a name into the `Saved Sessions`-field and pressing `Save`.
    3. Press `Open`.
3. Successful connection results in an empty PuTTY window.

## Program executable to device

We use Xilinx's tool `xsct` to program the executable to the device.
You can find this program e.g. by typing `Xilinx Software Command Line Tool` to Windows Start Menu.

1. Start `xsct`.
2. Navigate to project folder using `cd` (and `dir`).
3. Run Xilinx's provided `Tickle`-script.
    - `source run_on_pynq.tcl`

If everything went fine, the executable should print text to the PuTTY-terminal.

## Summary

After making changes to the source code, build the program.

`cargo build`

Run the executable on the device (using `xsct`-tool).

`source run_on_pynq.tcl`
