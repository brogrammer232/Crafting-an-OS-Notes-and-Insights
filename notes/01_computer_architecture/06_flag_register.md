# Flag Register

> **Random Quote:** The gap between a dream and reality is closed by showing up.

The **FLAG** register (**EFLAG** in 32-bit and **RFLAG** in 64-bit) is a special register that stores status bits reflecting the results of operations and controlling CPU behavior. It is read and updated automatically by the CPU based on arithmetic and control operations.

There are three (3) **categories of flags**:
1. [Status flags](#status-flags)
2. [Control flags](#control-flags)
3. [System flags](#system-flags)

### Status Flags
Status flags tell you about the result of the last operation. They reflect what happened after an instruction (like `ADD`, `SUB`, `CMP`, etc), and are essential for making decisions in code (like jumps).

These are the status flags: `CF`, `PF`, `AF`, `ZF`, `SF`, `OF`.

+ `CF`: **Carry Flag**. **BIT 0**. This flag is used in unsigned arithmetic (a number can only be zero or positive but not negative). It is set when one of two things happen:
    1. When an unsigned addition produces a carry out of the most significant bit (overflow). Example: When you try to add one to `0xFF`/`0b11111111`.
    2. When an unsigned subraction requires a borrow (i.e., the result goes below zero). Example: When you try to subract 2 from 1.

+ `PF`: **Parity Flag**. **BIT 2**. This flag is rarely used in modern code. It is set if the lowest byte of the result has an even number of 1s. Example: If `0b00001111` is the result, the parity flag will be set because there are 4 1s (even number of 1s). If `0b00001110` is the result, the parity flag will not be set because there are 3 1s (odd number of 1s).

+ `AF`: **Auxilliary Carry Flag**. **BIT 4**. This is another rarely used flag. It is set if there is a carry from bit 3 to bit 4. Example: Adding `0x01` to `0x0F` will result in `0x10`. There's a carry from bit 3 to 4 therefore, `AF` will be set. This is used in BCD (Binary-Coded Decimal) operations. Don't worry, you may never hear about this ever again. I hope I won't.

+ `ZF`: **Zero Flag**. **BIT 6**. This flag is very useful. It is set if the result of any operation is exactly zero. Example: Subtracting 3 from 3. The result is 0 therefore the zero flag will be set.

+ `SF`: **Sign Flag**. **BIT 7**. This flag is used with signed numbers (can be 0, negative or positive). It is set if the most significant bit (MSB) is 1. This means that the number is negative.

+ `OF`: **Overflow Flag**. **BIT 11**. This flag is used in signed arithmetic (a number can be 0, negative, or positive). This flag is set if the sign bit is wrong after an operation. If `positive + positive` results in a `negative`. Example: Adding 1 (`0x01`) to 127 (`0x7F`). The result is `0x80` which as a signed number is -128. In this case, the overflow flag will be set.

In your programs, you may tell the CPU to jump out of a loop when the zero flag is set (when a counter gets to 0), otherwise, jump to the top and keep looping (if the zero flag is not set).

> You may be having a hard time understanding this. Don't worry. Stop trying to understand everything. Just get what you can for now. The rest will fall in place later when you get more pieces of the puzzle. Keep pushing soldier. 

### Control Flags
These flags affect how the CPU behaves. They toggle CPU features.

These are the control flags: `IF`, `DF`, `TF`.

+ `IF`: **Interrupt Flag.** **BIT 9**. This flag controls whether the CPU responds to maskable interrupts. When set (`IF = 1`), the CPU will handle hardware interrupts (e.g. keyboard, timer). When not set (`IF = 0`), the CPU will ignore maskable interrupts. (*Maskable interrupts are interrupts that can be silenced and un-silenced. The opposite are interrupts that must be handled and cannot be ignored.*) You can set this to 0 (clear interrupts using the `CLI` instruction) when you want to perform critical operations without interruption.

+ `DF`: **Direction Flag.** **BIT 10**. This flag controls direction for string instructions like `MOVS`, `LODS`, etc. When not set, the CPU will process strings **forward** (increment index). When set, the CPU will process strings **backward** (decrement index). You set this flag using the `STD` instruction and clear it using the `CLD` instruction.

+ `TF`: **Trap Flag.** **BIT 8**. This flag enables "single-step" mode which is useful for debugging. When set (`TF = 1`), the CPU triggers **INT 1** after every instruction. This is used by debuggers like GDB to walk you through instruction by instruction.

### System Flags
System flags control low-level CPU behaviors and indicate special processor states. They are used by the operating system and virtualization layers to manage stuff like: interrupt behavior, debugging traps, virtual 8086 mode, etc.

These are the system flags: `RF`, `VM`, `AC`, `VIF`, `VIP`, `ID`.

+ `RF`: **Resume Flag**. **BIT 16**. This flag is used by debuggers to supress re-triggering of faults. When a debug exception (like a breakpoint) is hit, setting RF before `IRET` prevents the same instruction from causing another fault. This is used in the debug trap handler.

+ `VM`: **Virtual 8086 Mode**. **BIT 17**. This flag enables real-mode-like execution in protected mode. This is useful for running real-mode programs inside a 32-bit (protected mode OS).

+ `AC`: **Alignment Check**. **BIT 18**. When set, and in user mode, misaligned memory accesses (e.g. word on odd addresses) will throw #AC exceptions. This only works if CR0.AM (Alignment Mask) is also set. It is useful in debugging, but mostly disabled for performance.

+ `VIF`: **Virtual Interrupt Flag**. **BIT 19**. This flag is used by the **Virtual Machine Monitor (VMM)** to emulate IF (Interrupt Flag) in V8086 mode. This lets VMs simulate interrupts being enabled without giving them actual interrupt access.

+ `VIP`: **Virtual Interrupt Pending**. **BIT 20**. Indicates a virtual interrupt is pending, but IF is currently cleared. This lets the monitor know to deliver the interrupt later when IF is set.

+ `ID`: **ID Flag**. **BIT 21**. This flag allows software to execute the `CPUID` instruction if set. You can toggle this bit to check if a CPU supports `CPUID`.
