# Pointer Registers

> **Random Quote:** You won't always feel motivated. That's why you bring discipline to work, not just dreams.

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
