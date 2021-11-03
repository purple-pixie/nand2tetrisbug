# nand2tetrisbug
A minimum setup to reproduce what seems like a bug in the VMEmulator around static variables in Sys.jack

When assigning a value to static variables within Sys.init(), the assignments clobber other classes' statics

Compiling and running these files demonstrates the issue - 333 gets written to RAM[16] by Keyboard.init, before being overwritten by Sys.init()
When Sys.init() calls Sys.static_test() it doesn't overwrite but instead writes to the (seemingly correct) address (RAM[17])

My assumption is that the emulator does not consider Sys.init to be a true Class function that really "belongs" to the Sys class, it is just where the emulation begins directly. Because of this some "currently being compiled/executed Class" variable is not being assigned, so references to `static` do not know which class's static table they should be looking up, and just default to the top of the static area.
