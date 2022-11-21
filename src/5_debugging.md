## Debugging

Xilinx `xsct` [reference guide](https://docs.xilinx.com/v/u/en-US/ug1208-xsct-reference-guide) contains useful commands for debugging your executable.

Maybe most important commands are following.

- `stop` to stop the executable.
- `con` to continue execution.
- `rdd` to read target registers.
- `stp` and `stpi` to single-step instructions.
- `dis` to disassemble instructions.
- `mrd` to read memory address contents.
- `bpadd` to add a breakpoint.
- `bpremove` to remove a breakpoint.
- `bplist` to list all breakpoints.

Printing the name of the currently executed function or blinking a LED at a strategic location can help you to debug your executable by understanding the control flow.
