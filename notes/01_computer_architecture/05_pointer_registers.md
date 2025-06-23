# Pointer Registers

> **Random Quote:** You won't always feel motivated. That's why you bring discipline to work, not just dreams.

**Pointer registers** are CPU registers conventionally used to store memory addresses. They are essential for memory addressing, stack operations, function calls, and traversing data structures.

The main pointer registers are: `IP`, `SP`, `BP`, `SI`, and `DI`.

> **Note:** In protected mode, these become `EIP`, `ESP`, `EBP`, etc. In long mode, they are extended further to `RIP`, `RSP`, `RBP`, and so on.

---

### `IP` – Instruction Pointer

Holds the address of the next instruction to be executed.

* Cannot be modified directly with the `MOV` instruction.
* Can only be changed through control transfer instructions like `JMP`, `CALL`, `RET`, and through interrupts.

---

### `SP` – Stack Pointer

Points to the top of the stack.

* The stack grows downward in memory, meaning addresses decrease as data is pushed.
* This register is used by instructions such as `PUSH`, `POP`, `CALL`, and `RET`.
* You can set its value manually if needed.

---

### `BP` – Base Pointer

Used to access function parameters and local variables within the stack.

* Often used to set up a stack frame at the beginning of a function.
* Common pattern:

```assembly
MOV EBP, ESP        ; Set up the stack frame
MOV EAX, [EBP+8]    ; Access the first function argument
```

This technique will become clearer as you explore function calling conventions and stack layouts.

---

### `SI` and `DI` – Source Index and Destination Index

These registers are used for memory source and destination addressing.

* `SI` (or `ESI` / `RSI`) points to the source memory location.
* `DI` (or `EDI` / `RDI`) points to the destination memory location.
* Commonly used in string and memory operations such as `MOVS`, `LODS`, and `STOS`.
* In the x86\_64 calling convention, `RSI` and `RDI` are also used for passing the first and second function arguments, respectively.