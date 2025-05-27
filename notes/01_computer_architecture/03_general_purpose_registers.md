# General Purpose Registers

> **Random Quote:** Every line you write in pain teaches your future self fluency.

These registers are used for a lot of stuff including arithmetic, logic, data movement, etc. You'll use these registers most of the time.

They are:
+ `AX`: **The Accumulator.** This register is used for arithmetic, I/O, and return values. It is implicitly used in instructions like `MUL`, `DIV`, and I/O instructions like `IN` and `OUT`.
+ `BX`: **The base register.** It was originally used to hold base addresses for indirect memory access. You can use it for any general stuff like logic or arithmetic.
+ `CX`: **The count register.** This register is designed for loop control and shift/rotate operations. It is used as a counter for loop instructions like `LOOP` and `REP`.
+ `DX`: **The data register.** It is used in I/O, extended math, and sometimes as a second half for large values. It is essential when dealing with 32-bit or 64-bit results from 16-bit operations. Used by instructions like `IN` and `OUT` for I/O and `MUL` and `DIV` for math.

> **Note:** Other registers like the *pointer registers* and `R8` to `R15` registers in 64-bit are also classified as general-purpose registers. They are discussed in separate files.

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
