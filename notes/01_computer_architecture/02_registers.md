# Registers

> **Random Quote:** Some days you debug the code. Some days the code debugs you. Either way, you're learning.

**Registers** are small, ultra-fast memory locations built into the CPU. They store values the CPU is currently working with; like numbers, memory addresses, or instruction-related data. They are not general memory. You can't allocate them. You use them directly in assembly or indirectly through compiled code.

Each CPU core has its own set of the registers discussed below. This lets each one run its own thread, independently and in parallel without interfering with each other.

**Categories of registers:**
+ [General Purpose Registers](#general-purpose-registers)
+ [Segment Registers](#segment-registers)
+ [Pointer Registers](#pointer-registers)
+ [Flag Register](#flag-register)
+ [Control Registers](#control-registers)

## General Purpose Registers
These registers are used for a lot of stuff including arithmetic, logic, data movement, etc. You'll use these registers most of the time.

They are:
+ `AX`: **The Accumulator.** This register is used for arithmetic, I/O, and return values. It is implicitly used in instructions like `MUL`, `DIV`, and I/O instructions like `IN` and `OUT`.
+ `BX`: **The base register.** It was originally used to hold base addresses for indirect memory access. You can use it for any general stuff like logic or arithmetic.
+ `CX`: **The count register.** This register is designed for loop control and shift/rotate operations. It is used as a counter for loop instructions like `LOOP` and `REP`.
+ `DX`: **The data register.** It is used in I/O, extended math, and sometimes as a second half for large values. It is essential when dealing with 32-bit or 64-bit results from 16-bit operations. Used by instructions like `IN` and `OUT` for I/O and `MUL` and `DIV` for math.

> **Note:** Other registers like the *pointer registers* and `R8` to `R15` registers in 64-bit are also classified as general-purpose registers. They are discussed somewhere in this file separately.

Though each of these registers have specific uses, you can still use them for other things like logic. You can use `BX` instead of `CX` as a loop counter if you want. It's just easier to use `CX` instead. Also, some instructions expect you to use certain registers. For example, the `REP` instruction expects you to store the number of times to repeat in the `CX` register.

You may be wondering, "How much data can these registers hold?" Well, it depends. Let's focus on the `AX` register for now. The `AX` register is divided into multiple registers each with a symbolic name.
+ `AL`: **Low byte** of `AX`. Its size is 8 bits (1 byte).
+ `AH`: **High byte** of `AX`. Its size is also 8 bits.
+ `AX`: This is `AL` and `AH` combined. Its size is 16 bits (2 bytes). One from `AL` and the other from `AH`.
+ `EAX`: **Extended AX**. Its size is 32 bits (4 bytes).
+ `RAX`: **Extended EAX**. Its size is 64 bits (8 bytes).

This is the same for the other general purpose registers: `BX`, `CX`, `DX`.

**Key behavior to remember:**
+ Say you have written `0x1122334455667788` to `RAX`: `MOV RAX, 0x1122334455667788`.
+ Then you write `0xAABBCCDD` to `EAX`: `MOV EAX, 0xAABBCCDD`.
+ `RAX` will contain `0x00000000AABBCCDD`.

This will happen because, `MOV EAX, ...` zeroes out the upper 32 bits of `RAX`.

However, changing the other lower bits will not zero out the upper bits. Example:
+ If `EAX` contains `0xAABBCCDD`.
+ Then you write `0x1234` to `AX`.
+ `EAX` will contain `0xAABB1234`.
+ This behavior is the same for `AL` and `AH`. Writing to them affects only them.

Keep this in mind. Don't worry if you don't understand now. It will all fall into place as you proceed soldier.


## Segment Registers
**Segment registers** are 16-bit CPU registers used to define base addresses of memory segments. In the days before flat memory models, memory was divided into **segments**. These registers tell the CPU where each segment starts.

Segment registers are used differently in different CPU modes:
+ In **real mode**, segment registers are used for memory addressing. Real mode uses **segment:offset** addressing. This means that the CPU will use two values (the segment and the offset) to calculate the memory address. The value of the segment is stored in one of the segment registers and the value of the offset can be passed as a literal or in a general purpose register. I believe there is no need to go into the math of segment:offset addressing now. That'll come later.

+ In **protected mode**, segment registers hold selectors into the **GDT (Global Descriptor Table)** or **LDT (Local Descriptor Table)**. In a nut shell, each GDT entry tells the CPU: where the segment starts, how big it is, whether its code or data and what privilege level it's for. All this will make sense when learning how to switch from real mode to protected mode. For now, just know that in protected mode, segment registers hold selectors to the GDT or LDT.

+ In **long mode**, segment registers are *mostly* ignored. `CS` controls which code segment is executing. `FS/GS` are still used, often for thread-local storage. `DS`, `ES`, and `SS` are *flat* (base = 0, limit = `0xFFFFFFFF`.

These are the segment registers: `CS`, `DS`, `SS`, `ES`, `FS`, `GS`.
+ `CS`: **Code Segment.** This is the segment where code (instructions) are fetched from. In **real mode**, this register points to the code segment. In **protected mode**, it points to a GDT entry defining the executable segment. In **long mode**, the `CS` selector still matters for ring level and other stuff but the base/limit are ignored. The instruction pointer in `RIP` is treated as a flat pointer. The ring level is embedded in the two lowest bits of the selector in `CS`. (Keep going, it will all make sense later.)

+ `DS`: **Data Segment.** Where data variables reside. This register is used by default for accessing data, when no other segment override is used. In **real mode**, this register points to the data segment. In **protected mode**, the value in `DS` points to a GDT entry defining a readable data segment. In **long mode**, it's ignored. It usually has a base 0.

+ `SS`: **Stack Segment.** This segment defines where the stack resides. In **real mode**, it points to the stack segment. It is usually used together with `SP` and `BP` for segment:offset addressing, `SS` containing the segment and `SP` or `BP` having the offset. In **protected mode**, `SS` must point to a GDT entry with writable privileges. This is where the stack will be. In **long mode**, `SS` is ignored, treated as flat base 0.

+ `ES`: **Extra segment**. It is used implicitly by string operations like `MOVS`, `STOS`, `LODS`, etc. In **real mode**, it points to the segment containing the string for string operations (you set this manually). In **protected mode**, it points to a GDT entry defining a readable data segment. In **long mode**, it's ignored.

+ `FS` and `GS`: These are 2 more **extra segments**. In **real mode**, they are ignored. In **protected mode**, they point to GDT entries which define a base, limit, and access rights. They are commonly used for Thread-Local Storage (TLS) on Windows and Linux. In **long mode**, the CPU reads base addresses for `FS`/`GS` from Model-Specific Registers (MSRs). Linux uses `GS` for per-CPU data in the kernel.


## Pointer Registers
Pointer registers are registers that are conventionally used to hold memory addresses. They're crucial for addressing, stack operations, function calls, and memory traversal.

These are the pointer registers: `IP`, `SP`, `BP`, `SI`, `DI`.

> **Note:** In protected mode they are `EIP`, ... and in long mode they are `RIP`, ... .

+ `IP`: **Instruction Pointer.** This points to the next instruction to execute. It cannot be modified directly using the `MOV` instruction. You can only modify it using jumps, calls or interrupts.

+ `SP`: **Stack Pointer.** This points to the top of the stack. The stack grows downwards in memory (addresses decrease as stack grows). You can set this to whatever you want. It is used by instructions like `PUSH`, `POP`, `CALL` and `RET`.

+ `BP`: **Base Pointer.** This is used to access function arguments and local variables in the stack. Often used like this:
```assembly
;This will all make sense later.
MOV EBP, ESP        ;Set up stack frame.
MOV EAX, [EBP+8]    ;Get first function argument.
```

+ `SI` and `DI`: **Source Index** and **Destination Index**. `SI` points to memory source and `DI` points to memory destination. They are used in string/memory instructions like `MOVS`, `LODS` and `STOS`. They are also used for function parameters in x86\_64 clling convention.


## Flag Register
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


## Control Registers
Control registers are special CPU registers used to control low-level processor features like memory management, protection, and hardware access.

These registers can only be accessed in kernel mode (ring 0). Trying to access them in user mode results in a #GP (General Protection Fault).

These are the control registers: `CR0`, `CR2`, `CR3`, `CR4`, and `CR8`. I promise I didn't miss any. `CR1`, `CR5`, `CR6` and `CR7` were reserved for future expansion, but nothing has been implemented yet. They physically don't exist on the CPU. Reading or writing to them will trigger a #UD (Invalid Opcode Exception).

+ `CR0`: This register is used to enable protected mode, paging, and write protection.
    - **BIT 0**. **PE**. Setting this bit will enable 32-bit protected mode.
    - **BIT 31**. **PG**. Setting this bit will enable virtual memory paging.
    - **BIT 16**. **WP**. Setting this bit will enable write protect. This will protect read-only pages even in kernel mode.
    - **BIT 18**. **AM**. Alignment Mask. This is used together with `AC` flag for alignment checks.

+ `CR2`: **Page Fault Liner Address**. When a page fault (#PF) occurs, the CPU stores the address that caused it in `CR2`. The OS uses this to figure out what memory access failed and why.

+ `CR3`: **Page Directory Page Register (PDBR)**. This register points to the physical address of the page directory or PML4 (in x86\_64). When the OS switches page tables (e.g., during a context switch), it changes `CR3`. Also used by the CPU to cache translations in the TLB (Translation Lookaside Buffer). Changing `CR3` flushes the TLB.

+ `CR4`: This register is used to enable more advanced CPU features.
    - **BIT 5**. **PAE**. Setting this bit enables Physical Address Extension (36-bit addresses).
    - **BIT 9**. **OSFXSR**. This bit enables SSE instructions in OS (don't know what this means).
    - **BIT 13**. **VMXE**. This bit enables VMX (Intel VT-x virtualization).
    - **BIT 7**. **PGE**. Global Pages enabled (don't flush TLB on `CR3` switch).

+ `CR8`: **Task Priority Register (x86_64 only)**. This register controls the priority level for interrupts. It is used with APIC to mask lower-priority interrupts. This register is ignored in 32-bit mode.
