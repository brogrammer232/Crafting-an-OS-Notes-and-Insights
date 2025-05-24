# Registers

> **Random Quote:** Some days you debug the code. Some days the code debugs you. Either way, you're learning.

**Registers** are small, ultra-fast memory locations built into the CPU. They store values the CPU is currently working with; like numbers, memory addresses, or instruction-related data. They are not general memory. You can't allocate them. You use them directly in assembly or indirectly through compiled code.

**Categories of registers:**
+ [General Purpose Registers](#general-purpose-registers)
+ [Segment Registers](#segment-registers)
+ [Pointer Registers](#pointer-registers)
+ [Index Registers](#index-registers)
+ [Flags Registers](#flags-registers)
+ [Control Registers](#control-registers)

## General Purpose Registers
These registers are used for a lot of stuff including arithmetic, logic, data movement, etc. You'll use these registers most of the time.

They are:
+ `AX`: **The Accumulator.** This register is used for arithmetic, I/O, and return values. It is implicitly used in instructions like `MUL`, `DIV`, and I/O instructions like `IN` and `OUT`.
+ `BX`: **The base register.** It was originally used to hold base addresses for indirect memory access. You can use it for any general stuff like logic or arithmetic.
+ `CX`: **The count register.** This register is designed for loop control and shift/rotate operations. It is used as a counter for loop instructions like `LOOP` and `REP`.
+ `DX`: **The data register.** It is used in I/O, extended math, and sometimes as a second half for large values. It is essential when dealing with 32-bit or 64-bit results from 16-bit operations. Used by instructions like `IN` and `OUT` for I/O and `MUL` and `DIV` for math.

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


















