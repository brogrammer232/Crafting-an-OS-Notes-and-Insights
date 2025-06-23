# Control Registers

> **Random Quote:** Winners act, losers give excuses.

**Control registers** are special-purpose CPU registers used to manage core processor features such as memory protection, paging, hardware virtualization, and low-level access control.

These registers can only be accessed in **kernel mode (ring 0)**. Attempting to access them in **user mode** results in a **#GP (General Protection Fault)**.

The primary control registers are: `CR0`, `CR2`, `CR3`, `CR4`, and `CR8`.

> Note: `CR1`, `CR5`, `CR6`, and `CR7` do not exist. They are reserved for potential future use, but no hardware implementation has been provided. Any attempt to read or write to them will result in a **#UD (Invalid Opcode Exception).**

---

### `CR0` – Control Register 0

Used to enable or configure core features such as protected mode and virtual memory.

Important bits:

* **Bit 0 – PE (Protection Enable):** Enables 32-bit protected mode when set.
* **Bit 31 – PG (Paging):** Enables paging and virtual memory support when set.
* **Bit 16 – WP (Write Protect):** Enforces write protection for read-only pages, even in kernel mode.
* **Bit 18 – AM (Alignment Mask):** Works together with the `AC` flag in `RFLAGS` to enable alignment checking.

---

### `CR2` – Page Fault Linear Address

When a **page fault (#PF)** occurs, the CPU stores the **faulting virtual address** in `CR2`.
Operating systems use this register to diagnose and handle memory access violations.

---

### `CR3` – Page Directory Base Register (PDBR)

Points to the **physical address** of the current page directory in 32-bit mode or the **PML4** table in 64-bit mode.
When the operating system switches tasks or processes, it updates `CR3` to point to a new page table.

* Changing `CR3` causes the **Translation Lookaside Buffer (TLB)** to be flushed.
* This ensures that old virtual-to-physical address translations do not persist across context switches.

---

### `CR4` – Control Register 4

Used to enable and configure advanced CPU features.

Notable bits:

* **Bit 5 – PAE (Physical Address Extension):** Enables 36-bit physical addressing, which extends memory limits beyond 4 GB.
* **Bit 9 – OSFXSR:** Enables the use of **SSE (Streaming SIMD Extensions)** in operating systems.
* **Bit 13 – VMXE (VMX Enable):** Enables **Intel VT-x virtualization support**. Required for running virtual machines.
* **Bit 7 – PGE (Page Global Enable):** Allows global pages to persist across TLB flushes, improving performance during context switches.

---

### `CR8` – Task Priority Register (x86\_64 Only)

Used in **64-bit mode** to manage the interrupt priority level for **Advanced Programmable Interrupt Controllers (APICs)**.

* Allows the CPU to **mask** lower-priority interrupts below a given threshold.
* Ignored entirely in **32-bit mode**.