# Control Registers

> **Random Quote:** Winners act, losers give excuses.

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
