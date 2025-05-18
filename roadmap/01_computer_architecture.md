# Understanding basic computer architecture
> **Random Quote:** The CPU is the heart of the machine, learn its rhythm, and you can make it dance.

The OS developer needs to understand the fundamental components and behavior of modern computer systems. This section provides the essential architecture knowledge needed to build a simple kernel.

**Contents:**
+ [Role of the CPU](#role-of-the-cpu)
+ [Registers](#registers)
+ [Binary and Hexadecimal](#binary-and-hexadecimal)
+ [Volatile Memory](#volatile-memory)
+ [I/O and Interrupts](#io-and-interrupts)
+ [Processor Modes](#processor-modes)
+ [Buses](#buses)
+ [Summary](#summary)

## Role of the CPU
The Central Processing Unit (CPU) is the primary component responsible for executing instructions and coordinating the operations of a computer system.

**Key concept:**
+ The Fetch-Decode-Execute cycle.

## Registers
Registers are small, fast storage locations within the CPU used to hold data, addresses, or control information during program execution.

**Key registers:**
+ General purpose registers: `AX`, `BX`, `CX`, `DX`.
+ Segment registers: `CS`, `DS`, `SS`, `ES`, `FS`, `GS`.
+ Pointer and index registers: `SP`, `BP`, `SI`, `DI`.
+ Instruction pointer: `IP`.
+ Flags register: `FLAGS/EFLAGS`.

## Binary and Hexadecimal
The CPU speaks binary. Hexadecimal is an easier way of writing binary. OS development requires you to read and write fluent binary and hexadecimal.

**Key topics:**
+ Binary
+ Hexadecimal
+ Decimal
+ Converting between binary, hexadecimal and decimal
+ Bit masking

## Volatile Memory
Memory in computer systems is structured hierarchically and accessed by the CPU for code and data storage.

**Key topics:**
+ RAM vs registers
+ Segmentation and Addressing
+ Memory-mapped I/O (MMIO)

## I/O and Interrupts
The CPU communicates with hardware through I/O ports and handles asynchronous events using interrupts.

> *Don't go too deep on this. We'll revisit it when we get to the kernel.*

**Key topics:**
+ I/O ports
+ Interrupts
+ BIOS interrupts

## Processor Modes
x86 processors support multiple modes of operation. Understanding these modes is critical for writing a bootloader and kernel.

**Key topics:**
+ Real mode
+ Protected mode
+ Long mode

## Buses
Buses are communication pathways used by the CPU to interact with memory and peripherals.

+ Types of buses: address bus, data but, control but.

> *Keep this brief too.*
