# Segment Registers

> **Random Quote:** Consistency beats bursts of brilliance.

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
