# Understanding basic computer architecture

> **Random Quote:** The CPU is the heart of the machine, learn its rhythm, and you can make it dance.

This roadmap outlines the essential concepts of computer architecture that must be understood before beginning OS development. A solid grasp of these fundamentals is necessary before moving on to writing low-level code.

## CPU

This is covered [here](../notes/01_computer_architecture/01_cpu.md).

+ The Fetch-Decode-Execute cycle.
+ The boot sequence.
+ Processor modes.

## Registers

This is covered [here](../notes/01_computer_architecture/02_registers.md)

+ General purpose registers: `AX`, `BX`, `CX`, `DX`.
+ Segment registers: `CS`, `DS`, `SS`, `ES`, `FS`, `GS`.
+ Pointer and index registers: `SP`, `BP`, `SI`, `DI`.
+ Instruction pointer: `IP`.
+ Flags register: `FLAGS/EFLAGS`.
+ Control registers: `CR0`, `CR1`, `CR2`, `CR3`, `CR4`.

## Number Systems

This is covered [here](../notes/01_computer_architecture/08_number_systems.md)

+ Decimal
+ Binary
+ Hexadecimal
+ Converting between binary, hexadecimal and decimal

## Signed and Unsigned Numbers

This is covered [here](../notes/01_computer_architecture/09_signed_and_unsigned_numbers.md)

+ Signed Numbers
+ Unsigned Numbers
+ Integer Representation (two's complement) (touch on the others).

## Number Systems: Arithmetic

This is covered [here](../notes/01_computer_architecture/10_number_systems_arithmetic.md)

+ Binary arithmetic
+ Hexadecimal arithmetic

## Bitwise Operations

This is covered [here](../notes/01_computer_architecture/13_bitwise_operations.md)

+ AND, OR, XOR, NOT
+ Bitwise shifts (left shift, right shift)

## Endianness

This is covered [here](../notes/01_computer_architecture/14_endianness.md)

+ Big-endian vs Little-endian
+ Reading multi-byte values in memory

## Volatile and Non-Volatile Memory

+ RAM vs ROM
+ Difference in read/write speeds
+ Use cases

## Memory Addressing

+ Segmentation and Offset.
+ Memory regions (`0x0000` - `0xFFFF`)
+ Real mode memory map (`0x7C00`)

## Calculating Memory Sizes

+ Calculating size of memory ranges (e.g. `0x7C00` to `0x7CFF`)
+ Understanding kilobytes, megabytes, etc, in binary form (1 KiB = 1024 bytes).

## Memory-Mapped I/O (MMIO)

+ Why devices appear as memory addresses.
+ Why `0xB8000` maps to video memory in text mode.
+ Early interaction with screen/keyboard via memory.

## Memory Layout in real mode

+ 1 MiB addressable space.
+ Conventional memory, upper memory, BIOS, etc.
+ How the bootloader fits into this.
