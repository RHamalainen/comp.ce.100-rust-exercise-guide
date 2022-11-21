## LED-array

Indexing the leds-array works a little different between C and Rust.

### C

The C-version of the course project declares a global volatile three-dimensional array like so:

`volatile uint8_t dots[8][8][3] = {0};`

Volatile means that the values of the table are prone to change by something other than the code that is currently executing close by.
An instance of such a situation would be when two different interrupt handlers are simultaneously reading from and writing into the array.

### Rust

In Rust, you can omit `volatile` since Rust handles that functionality on access of a binding, not at declaration of the variable like in C.
A similar three-dimensional array would be declared like so:

`static mut dots: [[[u8; 3]; 8]; 8] = [[[0; 3]; 8]; 8];`

While the indexing looks different, the meaning is the same: there's two 8-length outer dimensions for the x- and y-coordinates, and the innermost dimension for the color with a length of three.

Indexing the first color (perhaps red) at "x = 1", "y = 2" would then in both languages be expressed as `dots[1][2][0]`.

With this knowledge, you might be able to re-write `SetPixel` from the original C code.
