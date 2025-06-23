# Signed and Unsigned Numbers

> **Random Quote:** The computer is incredibly fast, accurate, and stupid. Man is unbelievably slow, inaccurate, and brilliant. The marriage of the two is a force beyond calculation.

## Unsigned Numbers

An **unsigned number** is a binary number that can only represent zero and positive values. All bits contribute to the magnitude. There is no sign bit.

**Range:**

| Bits | Min | Max                        |
| ---- | --- | -------------------------- |
| 8    | 0   | 255                        |
| 16   | 0   | 65,535                     |
| 32   | 0   | 4,294,967,295              |
| 64   | 0   | 18,446,744,073,709,551,615 |

**Binary representation:**
Straightforward mapping:

* `0` = `00000000`
* `1` = `00000001`
* `255` = `11111111`

**Overflow:**
Overflow occurs when a result exceeds the representable range. In unsigned types, overflow wraps around.

```c
uint8_t x = 255;
x += 1; // x becomes 0
```

**Why use unsigned numbers:**

* The value will never be negative (e.g., array size, memory address).
* Wider positive range than signed types of the same size.
* More natural in hardware-level programming.

**Things to watch out for:**

* Mixing `signed` and `unsigned` can lead to bugs:

  ```c
  int a = -1;
  unsigned int b = 1;

  if (a < b) // FALSE. a is converted to unsigned: very large value > 1.
  ```
* Subtraction can wrap:

  ```c
  unsigned int x = 5 - 10; // x = 4294967291 (if 32-bit)
  ```

---

## Signed Numbers

A **signed number** can represent both negative and positive values. The most significant bit (MSB) is used for the sign:

* `0` = positive
* `1` = negative

That leaves fewer bits for the magnitude compared to unsigned numbers.

**Range:**

| Bits | Min            | Max            |
| ---- | -------------- | -------------- |
| 8    | -128           | +127           |
| 16   | -32,768        | +32,767        |
| 32   | -2,147,483,648 | +2,147,483,647 |

**Binary representation:**
Signed numbers can be represented using:

* [Two's complement](#twos-complement)
* [One's complement](#ones-complement)
* [Sign-magnitude](#sign-magnitude)
* [Excess-K](#excess-k)

Most systems use two's complement.

**Overflow:**
Signed overflow wraps too, but it's **undefined behavior** in C/C++:

```c
int8_t x = 127;
x += 1; // wraps to -128
```

Binary:

```
127 = 01111111
+1  = 00000001
     ---------
     10000000 = -128 (in two's complement)
```

Same goes for negative overflow:

```c
int8_t x = -128;
x -= 1; // wraps to +127
```

**Why use signed numbers:**

* Required when working with negative values.

**Things to watch out for:**

* Comparing signed and unsigned values:

  ```c
  int a = -1;
  unsigned int b = 1;

  if (a < b) // FALSE. a becomes a large unsigned value.
  ```
* Signed overflow is undefined behavior in C/C++. Never rely on it.
* Be consistent with types. Don't mix signed and unsigned unless you're 100% sure of what you're doing.

---

## Signed Number Representation

### Two's Complement

Two's complement is the standard way computers represent signed integers.

**How it works:**

* Positive numbers are stored like unsigned.
* Negative numbers: invert the bits and add 1.

Example:

* `+5`: `00000101`
* `-5`:

  * Invert: `11111010`
  * Add 1: `11111011`

**Arithmetic example:**

```
 5 = 00000101
-3 = 11111101

Add:
    00000101
  + 11111101
  ----------
   100000010

Ignore the carry. Result: 00000010 = 2
```

**Why ignore the overflow?**
Because the result bits already hold the correct value. Two's complement makes arithmetic work naturally.

**Why it's used:**

* Same logic for both positive and negative arithmetic.
* Only one zero.
* Overflow wraps cleanly.
* Works perfectly with basic adders and ALUs.

**Where it's used:**

* Every modern CPU
* Compilers
* Operating systems
* Embedded systems

---

### One's Complement

Similar to two's complement, but without the "+1".

**How it works:**

* Negative = invert all bits.

Example:

* `+5`: `00000101`
* `-5`: `11111010`

**Problems:**

* Two zeroes: `00000000` and `11111111`
* Complicated arithmetic logic

**Why it's not used:**
It was replaced by two's complement. Don’t waste brain cells memorizing this.

---

### Sign-Magnitude

The human-friendly format.

**How it works:**

* MSB = sign (0 = positive, 1 = negative)
* Remaining bits = magnitude

Example:

* `+3`: `00000011`
* `-3`: `10000011`

**Problems:**

* Two zeros: `00000000` and `10000000`
* Complex arithmetic hardware

**Why it's not used:**
Simple for humans, awful for CPUs.

---

### Excess-K (Biased Representation)

Used for floating-point numbers.

**How it works:**

* Store: value + bias

**Example (Excess-127):**

* Bias = 127
* Actual -2 = Stored 125 = `01111101`
* Actual 0 = Stored 127 = `01111111`

**Why it's used:**

* Makes comparison fast and simple
* Simplifies floating-point logic

**Where it's used:**

* IEEE 754 (32-bit and 64-bit floats)
* CPU floating-point units
* Emulators and compilers