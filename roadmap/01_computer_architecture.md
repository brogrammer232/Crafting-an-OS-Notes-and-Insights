# Understanding Basic Computer Architecture

> **Random Quote:** The CPU is the heart of the machine. Learn its rhythm, and you can make it dance.

This roadmap outlines the essential concepts of computer architecture that should be understood before starting operating system development. A strong understanding of these basics is important before working with low-level code.

## CPU

Covered in: [CPU Overview](../notes/01_computer_architecture/01_cpu.md).

+ The fetch-decode-execute cycle.
+ The boot sequence.
+ Processor modes.

## Registers

Covered in: [Registers](../notes/01_computer_architecture/02_registers.md)

+ General-purpose registers: `AX`, `BX`, `CX`, `DX`.
+ Segment registers: `CS`, `DS`, `SS`, `ES`, `FS`, `GS`.
+ Pointer and index registers: `SP`, `BP`, `SI`, `DI`.
+ Instruction pointer: `IP`.
+ Flags register: `FLAGS/EFLAGS`.
+ Control registers: `CR0`, `CR1`, `CR2`, `CR3`, `CR4`.

## Number Systems

Covered in: [Number Systems](../notes/01_computer_architecture/08_number_systems.md)

+ Decimal
+ Binary
+ Hexadecimal
+ Converting between binary, hexadecimal and decimal

## Signed and Unsigned Numbers

Covered in: [Signed and Unsigned Numbers](../notes/01_computer_architecture/09_signed_and_unsigned_numbers.md)

+ Signed Numbers
+ Unsigned Numbers
+ Integer Representation (two's complement, and brief mention of other methods).

## Number Systems: Arithmetic

Covered in: [Arithmetic in Number Systems](../notes/01_computer_architecture/10_number_systems_arithmetic.md)

+ Binary arithmetic
+ Hexadecimal arithmetic

## Bitwise Operations

Covered in: [Bitwise Operations](../notes/01_computer_architecture/13_bitwise_operations.md)

+ AND, OR, XOR, NOT
+ Bitwise shifts: left shift, right shift

## Endianness

Covered in: [Endianness](../notes/01_computer_architecture/14_endianness.md)

+ Big-endian vs little-endian
+ Reading multi-byte values in memory

## Volatile and Non-Volatile Memory

Covered in: [Volatile and Non-Volatile Memory](../notes/01_computer_architecture/15_volatile_and_non_volatile_memory.md)

+ RAM vs ROM
+ Difference in read/write speeds
+ Common use cases
