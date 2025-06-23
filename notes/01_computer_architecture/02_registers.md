# Registers

> **Random Quote:** Some days you debug the code. Some days the code debugs you. Either way, you're learning.

**Registers** are small, high-speed memory units built directly into the CPU. They store values that the CPU is actively using, such as numbers, memory addresses, or instruction-related data. Registers are not general-purpose memory, you cannot allocate or manage them like RAM. Instead, they are accessed directly in assembly language or indirectly through compiled code.

Each CPU core has its own dedicated set of the registers discussed below. This design allows multiple cores to execute separate threads independently and in parallel, without interfering with one another.

**Categories of Registers:**

* [General Purpose Registers](./03_general_purpose_registers.md)
* [Segment Registers](./04_segment_registers.md)
* [Pointer Registers](./05_pointer_registers.md)
* [Flag Register](./06_flag_register.md)
* [Control Registers](./07_control_registers.md)