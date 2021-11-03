# nand2tetrisbug
A minimum setup to reproduce what seems like a bug in the VMEmulator around static variables in Sys.jack

When assigning a value to static variables within Sys.init(), the assignments clobber other classes' statics

Compiling and running these files demonstrates the issue - 333 gets written to RAM[16] by Keyboard.init, before being overwritten by Sys.init()
When Sys.init() calls Sys.static_test() it doesn't overwrite but instead writes to the (seemingly correct) address (RAM[17])
