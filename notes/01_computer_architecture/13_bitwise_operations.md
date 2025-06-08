# Bitwise Operations

> **Random Quote:** First, solve the problem. Then, write the code.

Bitwise operations act directly on the binary representations of integers. Each operation manipulates bits independently.

---

## Key Topics

- [AND (`&`)](#and-)
- [OR (`|`)](#or-)
- [XOR (`^`)](#xor-)
- [NOT (`~`)](#not-)
- [Right Shift (`>>`)](#right-shift-)
- [Left Shift (`<<`)](#left-shift-)

---

## AND (`&`)

Bitwise AND (`&`) returns `1` if **both bits** are `1`.

| A | B | A & B |
| - | - | ----- |
| 0 | 0 | 0     |
| 0 | 1 | 0     |
| 1 | 0 | 0     |
| 1 | 1 | 1     |

**Example:**

```C
int a = 0b1100; // 12
int b = 0b1010; // 10
int result = a & b; // 0b1000 (8)
```

---

## OR (`|`)

Bitwise OR (`|`) returns `1` if **at least one bit** is `1`.

| A | B | A \| B |
| - | - | ------ |
| 0 | 0 | 0      |
| 0 | 1 | 1      |
| 1 | 0 | 1      |
| 1 | 1 | 1      |

**Example:**

```C
int a = 0b1100;
int b = 0b1010;
int result = a | b; // 0b1110 (14)
```

---

## XOR (`^`)

Bitwise XOR (`^`) returns `1` if **the bits are different**.

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

---

## NOT (`~`)

Bitwise NOT (`~`) flips every bit (`1 -> 0`, `0 -> 1`).

**Example:**

```c
int a = 0b00001111; // 15
int result = ~a;    // 0b11110000, but in 32-bit signed: -16
```

> **Note:** `~` is a **unary operator**, and in C, it affects the sign bit in signed integers.

### Bonus: Operator Types

+ **Unary operators:** Takes **one operand**. Example: `~a`.
+ **Binary operators:** Takes **two operands**. Example: `a & b`.
+ **Ternary operators:** Takes **three operands**. Example:

```c
int max = (a > b) ? a : b;
```

Operands:

1. Condition: `a > b`
2. Value if true: `a`
3. Value if false: `b`

---

## Right Shift (`>>`)

Right shift (`>>`) shifts bits to the right.

+ `a >> n` is equivalent to `a / 2^n`.

There are 2 kinds of right shifts:

+ **Logical shift:** For **unsigned values**. Fills in `0`s on the left.
+ **Arithmetic shift:** For **signed values**. Fills in the sign bit on the left.

### Logical Shift Examples

```c
unsigned int x = 8;   // binary: 0000 1000
unsigned int y = x >> 1; //       0000 0100 => 4
```

+ `8 >> 1` = `8 / 2^1 = 4`.

```c
unsigned int x = 20;  // binary: 0001 0100
unsigned int y = x >> 2; //       0000 0101 => 5
```

### Arithmetic Shift Examples

```c
int x = -4;         // In binary (2’s comp): 1111 1100
int y = x >> 1;     // Result: 1111 1110 => -2
```

+ Arithmetic shift **preserves the sign** by filling the left with the sign bit.

---

## Left Shift (`<<`)

Left shift (`<<`) shifts bits to the left and fills in `0`s on the right.

+ `a << n` is equivalent to `a * 2^n`. Each left shift by 1 is equivalent to multiplying by `2^1`.


### Examples

```c
int a = 0b00000001; // 1
int result = a << 3; // 0b00001000 (8)
```

+ `a << 3` = `a * 2^3` (`2^3 = 8`)

```c
int x = 0b00000011; // 3
int y = x << 1; // 00000110 = 6
```

+ `3 << 1` = `3 * 2^1` (`2^1 = 2`)

### Overflow risk

+ Left shifting large values can cause overflow, especially on fixed-size types like `uint8_t`.

```c
uint8_t x = 200;    // binary: 1100 1000
uint8_t y = x << 1; //        1001 0000 => 144
```