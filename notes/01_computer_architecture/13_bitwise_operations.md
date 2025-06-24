# Bitwise Operations

> **Random Quote:** First, solve the problem. Then, write the code.

Bitwise operations manipulate individual bits within binary numbers. They're the bread and butter of low-level programming, system calls, flags, masks, and hardware hacking.

---

## Key Topics

* [AND (`&`)](#and-)
* [OR (`|`)](#or-)
* [XOR (`^`)](#xor-)
* [NOT (`~`)](#not-)
* [Right Shift (`>>`)](#right-shift-)
* [Left Shift (`<<`)](#left-shift-)

---

## AND (`&`)

Returns `1` if **both** bits are `1`. Otherwise, `0`.

| A | B | A & B |
| - | - | ----- |
| 0 | 0 | 0     |
| 0 | 1 | 0     |
| 1 | 0 | 0     |
| 1 | 1 | 1     |

**Example:**

```c
int a = 0b1100; // 12
int b = 0b1010; // 10
int result = a & b; // 0b1000 (8)
```

Used often for **masking** bits.

---

## OR (`|`)

Returns `1` if **at least one** bit is `1`.

| A | B | A \| B |
| - | - | ------ |
| 0 | 0 | 0      |
| 0 | 1 | 1      |
| 1 | 0 | 1      |
| 1 | 1 | 1      |

**Example:**

```c
int a = 0b1100;
int b = 0b1010;
int result = a | b; // 0b1110 (14)
```

Used to **set** specific bits.

---

## XOR (`^`)

Returns `1` if the bits are **different**.

| A | B | A ^ B |
| - | - | ----- |
| 0 | 0 | 0     |
| 0 | 1 | 1     |
| 1 | 0 | 1     |
| 1 | 1 | 0     |

**Example:**

```c
int a = 0b1100;
int b = 0b1010;
int result = a ^ b; // 0b0110 (6)
```

Used for **bit flipping**, **toggling**, and some crypto ops.

---

## NOT (`~`)

Unary operator that **flips** each bit. `1` becomes `0`, and vice versa.

**Example:**

```c
int a = 0b00001111; // 15
int result = ~a;    // 0b11110000 (in 32-bit signed: -16)
```

> **Note:** In C, `~` also flips the sign bit. So on signed types, you'll get negative results.

---

### Quick Operator Reference

| Type    | Arity | Example           |
| ------- | ----- | ----------------- |
| Unary   | 1     | `~a`              |
| Binary  | 2     | `a & b`, `a << b` |
| Ternary | 3     | `(a > b) ? a : b` |

---

## Right Shift (`>>`)

Shifts bits **to the right**.

* `a >> n` is roughly `a / 2^n`

Two behaviors:

* **Logical shift:** for unsigned types. Fills with 0s.
* **Arithmetic shift:** for signed types. Fills with the **sign bit**.

### Logical shift examples:

```c
unsigned int x = 8;       // 00001000
unsigned int y = x >> 1;  // 00000100 => 4
```

```c
unsigned int x = 20;      // 00010100
unsigned int y = x >> 2;  // 00000101 => 5
```

### Arithmetic shift:

```c
int x = -4;        // 11111100 (in two's complement)
int y = x >> 1;    // 11111110 => -2
```

---

## Left Shift (`<<`)

Shifts bits **to the left**, inserting `0`s from the right.

* `a << n` is `a * 2^n`

### Examples:

```c
int a = 0b00000001;     // 1
int result = a << 3;    // 0b00001000 (8)
```

```c
int x = 0b00000011;     // 3
int y = x << 1;         // 0b00000110 = 6
```

---

### Watch out: Overflow

Left shifting large values can overflow and wrap around silently.

```c
uint8_t x = 200;     // 11001000
uint8_t y = x << 1;  // 10010000 => 144 (wrapped)
```

---
