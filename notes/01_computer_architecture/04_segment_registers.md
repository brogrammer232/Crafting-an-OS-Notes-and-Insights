# Segment Registers

> **Random Quote:** Consistency beats intensity.

**Segment registers** are 16-bit CPU registers used to define the base addresses of memory segments. In the early days of computing, before flat memory models became standard, memory was divided into **segments**. Segment registers tell the CPU where each of these segments begins.

Their role and behavior depend on the CPU mode in use:

* In **real mode**, segment registers are used for memory addressing through **segment\:offset** addressing. The CPU calculates memory addresses using two values: the segment (stored in a segment register) and the offset (either a literal or a value in a general-purpose register). The actual math behind this will be covered later.

* In **protected mode**, segment registers contain **selectors** that index into the **Global Descriptor Table (GDT)** or **Local Descriptor Table (LDT)**. Each GDT entry describes the base address, size, type (code or data), and privilege level of a segment. This concept will make more sense when you learn how to transition from real mode to protected mode.

* In **long mode**, most segment registers are ignored. The `CS` register still determines the currently executing code segment. The `FS` and `GS` registers remain in use, often for thread-local storage. Registers like `DS`, `ES`, and `SS` are treated as flat segments with a base address of 0 and a limit of `0xFFFFFFFF`.

---

## The Segment Registers

The CPU provides six segment registers: `CS`, `DS`, `SS`, `ES`, `FS`, and `GS`.

---

### `CS` – Code Segment

This register defines the segment from which the CPU fetches instructions.

* **In real mode:** Points to the code segment.
* **In protected mode:** Contains a selector referencing a GDT entry for an executable segment.
* **In long mode:** Still significant. The base and limit are ignored, but the selector controls the privilege level. The `RIP` register is used as a flat pointer. The ring level is embedded in the two lowest bits of the `CS` selector. This will become clearer later.

---

### `DS` – Data Segment

Used by default when accessing data, unless another segment is specified.

* **In real mode:** Points to the segment holding general data.
* **In protected mode:** Contains a selector referencing a GDT entry for a readable data segment.
* **In long mode:** Ignored by the CPU. The base is usually 0.

---

### `SS` – Stack Segment

Defines the segment used for the stack.

* **In real mode:** Points to the stack segment and works with `SP` or `BP` for segment\:offset addressing.
* **In protected mode:** Must reference a GDT entry with write permissions. This segment is used by stack operations.
* **In long mode:** Ignored by the CPU and treated as a flat segment with base 0.

---

### `ES` – Extra Segment

Used implicitly by string instructions like `MOVS`, `STOS`, and `LODS`.

* **In real mode:** Manually set to point to the relevant string data segment.
* **In protected mode:** Contains a selector referencing a readable data segment.
* **In long mode:** Ignored by the CPU.

---

### `FS` and `GS` – Additional Extra Segments

These are additional segment registers used primarily for specialized purposes.

* **In real mode:** Ignored.
* **In protected mode:** Contain selectors referencing GDT entries with specific base addresses and access rights. Commonly used for Thread-Local Storage (TLS).
* **In long mode:** The CPU obtains base addresses for `FS` and `GS` from Model-Specific Registers (MSRs). Linux, for example, uses `GS` for per-CPU kernel data.