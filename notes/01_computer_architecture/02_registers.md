# Registers

> **Random Quote:** Some days you debug the code. Some days the code debugs you. Either way, you're learning.

**Registers** are small, ultra-fast memory locations built into the CPU. They store values the CPU is currently working with; like numbers, memory addresses, or instruction-related data. They are not general memory. You can't allocate them. You use them directly in assembly or indirectly through compiled code.

Each CPU core has its own set of the registers about to be discussed. This lets each one run its own thread, independently and in parallel without interfering with each other.

**Categories of registers:**
+ [General Purpose Registers](./03_general_purpose_registers.md)
+ [Segment Registers](./04_segment_registers.md)
+ [Pointer Registers](./05_pointer_registers.md)
+ [Flag Register](./06_flag_register.md)
+ [Control Registers](./07_control_registers.md)