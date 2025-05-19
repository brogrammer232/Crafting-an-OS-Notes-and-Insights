# BIOS Interrupts

**Random Quote:** Before the OS takes the wheel, the BIOS hands you the keys.

BIOS interrupts provide a standardized interface to hardware services during early boot, before the operating system takes full control. Learning how to use them is essential for writing real-mode bootloaders and early diagnostics.

## What are BIOS interrupts
Learn:
+ What they are and why they exist.
+ The limitations and advantages.
+ The calling conventions; input and output through registers.

+ Revisit the `INT` instruction.

## Common BIOS interrupts
There are tons of BIOS interrupts. You don't have to learn all of them at once. Here's a list of the most useful and common ones.
+ `INT 0x10`: Video services (print characters, VGA graphics, etc).
+ `INT 0x13`: Disk services (read/write sectors).
+ `INT 0x16`: Keyboard input.

Optional:
+ `INT 0x15`: System services (advanced features).
+ `INT 0x1A`: Real-time clock and date functions.
