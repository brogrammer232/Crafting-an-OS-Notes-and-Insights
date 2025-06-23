# General Purpose Registers

> **Random Quote:** Every line you write in pain teaches your future self fluency.

General purpose registers are used for a wide range of operations, including arithmetic, logic, data movement, and more. These are the registers you will use most frequently in assembly programming.

The main general purpose registers are:

* **`AX` – The Accumulator:**
  Commonly used for arithmetic operations, input/output, and storing return values. It is implicitly used by instructions such as `MUL`, `DIV`, and I/O operations like `IN` and `OUT`.

* **`BX` – The Base Register:**
  Originally intended to store base addresses for indirect memory access. It can also be used for general logic and arithmetic operations.

* **`CX` – The Count Register:**
  Designed for use in loop control and shift/rotate operations. It acts as a counter in instructions like `LOOP` and `REP`.

* **`DX` – The Data Register:**
  Used in I/O operations, extended arithmetic, and as a second half when handling large values. It is essential in 16-bit operations producing 32-bit or 64-bit results, such as with `MUL`, `DIV`, `IN`, and `OUT`.

> **Note:** Other registers, such as pointer registers and `R8` to `R15` in 64-bit systems, are also considered general-purpose registers. They are covered in separate sections.

Although these registers have traditional roles, they are interchangeable in many contexts. For instance, you can use `BX` instead of `CX` as a loop counter, although some instructions expect specific registers. For example, the `REP` instruction expects the count to be placed in `CX`.

---

## Register Sizes and Hierarchy

You may be wondering how much data each register can hold. Let’s examine the `AX` register as an example. It is actually a part of a hierarchy of registers, each representing a different portion of the full 64-bit register:

* `AL`: The **low byte** of `AX` – 8 bits (1 byte)
* `AH`: The **high byte** of `AX` – 8 bits
* `AX`: Combination of `AH` and `AL` – 16 bits (2 bytes)
* `EAX`: **Extended AX** – 32 bits (4 bytes)
* `RAX`: **Extended EAX** – 64 bits (8 bytes)

This structure is consistent across other general-purpose registers like `BX`, `CX`, and `DX`.

---

## Important Behavior

Consider the following example:

```asm
MOV RAX, 0x1122334455667788
MOV EAX, 0xAABBCCDD
```

After these instructions execute, `RAX` will contain:

```text
0x00000000AABBCCDD
```

This happens because writing to `EAX` **zeroes out the upper 32 bits** of `RAX`.

However, writing to lower parts of the register, such as `AX`, `AL`, or `AH`, **does not** clear the upper bits. For example:

```asm
MOV EAX, 0xAABBCCDD
MOV AX, 0x1234
```

Now, `EAX` will contain:

```text
0xAABB1234
```

Similarly, writing to `AL` or `AH` only affects those respective 8-bit portions.

---

Keep this behavior in mind. Don’t worry if it seems confusing at first, it will make more sense as you continue learning. Stay persistent, soldier.