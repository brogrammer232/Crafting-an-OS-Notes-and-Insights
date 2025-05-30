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
This is covered [here](../notes/01_computer_architecture/09_signed_and_unsigned_numbers)
+ Signed Numbers
+ Unsigned Numbers
+ Integer Representation (two's complement) (touch on the others).

## Number Systems: Arithmetic
+ Binary arithmetic
+ Hexadecimal arithmetic

## Bitwise Operations
+ AND, OR, XOR, NOT
+ Bitwise shifts (left shift, right shift) (logical vs arithmetic)

## Endianness
+ Big-endian vs Little-endian
+ Reading multi-byte values in memory

## Volatile Memory
+ RAM vs registers
+ Segmentation and Addressing
+ Memory-mapped I/O (MMIO)

## Interrupts
+ System Interrupts
+ BIOS interrupts
+ Hardware interrupts
