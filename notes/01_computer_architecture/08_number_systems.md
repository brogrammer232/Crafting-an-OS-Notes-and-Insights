# Number Systems

> **Random Quote:** You don't have to see the whole system call, just the next instruction.

A **number system** is a standardized way to represent and interpret numeric values. In operating system development, the most commonly used number systems are:

* [Decimal](#decimal)
* [Binary](#binary)
* [Hexadecimal](#hexadecimal)

It is essential to understand how to convert between these systems:

* [Decimal and Binary](#decimal-and-binary)
* [Decimal and Hexadecimal](#decimal-and-hexadecimal)
* [Binary and Hexadecimal](#binary-and-hexadecimal)

---

## Decimal

The **decimal system** is the standard number system used in everyday life (e.g., 1, 3, 35).

**Base:** It operates on **base 10**, meaning each digit represents a power of 10 based on its position. From right to left (starting at position 0), the total value is the sum of each digit multiplied by 10 raised to its position.

**Example:**

```
745 = (7 × 10²) + (4 × 10¹) + (5 × 10⁰) = 700 + 40 + 5  
 82 = (8 × 10¹) + (2 × 10⁰) = 80 + 2
```

**Usage:** Decimal is primarily used for display purposes or for input from the user. Internally, computers do not use decimal values.

**In code:** Decimal values are written normally.

```assembly
INC AX, 8   ; 8 is a decimal number
```

---

## Binary

The **binary system** is the native language of computers. It uses only two symbols: `0` and `1`, representing off/on or false/true states.

**Base:** Binary uses **base 2**. Each digit (called a bit) represents a power of 2.

**Example:**

```
100 = (1 × 2²) + (0 × 2¹) + (0 × 2⁰) = 4
```

**Usage:** The CPU performs all operations in binary. Binary data maps directly to machine instructions, registers, memory layouts, flags, and more.

**In code:** Binary values are prefixed with `0b`.

```assembly
MOV EAX, 0b10110001  ; This represents the binary number 10110001
```

---

## Hexadecimal

The **hexadecimal system** is a compact and human-readable representation of binary. It uses 16 digits: `0–9` and `A–F`, where:

* A = 10
* B = 11
* C = 12
* D = 13
* E = 14
* F = 15

**Base:** Hexadecimal operates on **base 16**. Each digit represents a power of 16.

**Example:**

```
2F = (2 × 16¹) + (15 × 16⁰) = 32 + 15 = 47
```

**Usage:** Hexadecimal is used extensively in system programming: memory addresses, binary dumps, disassembled code, and register values are often displayed in hex.

**In code:** Hex values are usually written with a `0x` prefix.

```assembly
MOV AX, 0xB8000  ; VGA text buffer starts here
```

Some assemblers allow a suffix style:

```assembly
MOV EAX, B8000h  ; Same as 0xB8000
```

---

## Conversions

### Decimal and Binary

**1. Decimal to Binary**

* Divide the number by 2 until the result is 0.
* Record the remainders.
* Read the remainders in reverse order (bottom to top).

**Example: Convert 13 to binary**

```
13 / 2 = 6, remainder 1  
 6 / 2 = 3, remainder 0  
 3 / 2 = 1, remainder 1  
 1 / 2 = 0, remainder 1

Binary: 1101
```

**2. Binary to Decimal**

* Multiply each bit by `2ⁿ` (where `n` is the position from right, starting at 0).
* Add the results.

**Example: Convert 1101 to decimal**

```
1101 = (1 × 2³) + (1 × 2²) + (0 × 2¹) + (1 × 2⁰)  
     = 8 + 4 + 0 + 1 = 13
```

---

### Decimal and Hexadecimal

**1. Decimal to Hexadecimal**

* Divide the number by 16 until the result is 0.
* Record remainders using hex notation.
* Read from bottom up.

**Example: Convert 430 to hexadecimal**

```
430 / 16 = 26, remainder 14 (E)  
 26 / 16 = 1, remainder 10 (A)  
  1 / 16 = 0, remainder 1

Hexadecimal: 0x1AE
```

**2. Hexadecimal to Decimal**

* Multiply each hex digit by `16ⁿ`, based on its position.
* Add the results.

**Example: Convert 0x1AE to decimal**

```
(1 × 16²) + (10 × 16¹) + (14 × 16⁰)  
= 256 + 160 + 14 = 430
```

---

### Binary and Hexadecimal

**1. Binary to Hexadecimal**

* Group the binary into 4-bit chunks (from right to left).
* Convert each group to hexadecimal.

**Example: Convert 0b10111110 to hexadecimal**

```
1011 = B  
1110 = E

Hexadecimal: 0xBE
```

You can convert using:

1. Binary -> Decimal -> Hexadecimal
2. A reference table (shown below)

**Binary to Hex Table:**

| Hex | Binary |
| --- | ------ |
| 0   | 0000   |
| 1   | 0001   |
| 2   | 0010   |
| 3   | 0011   |
| 4   | 0100   |
| 5   | 0101   |
| 6   | 0110   |
| 7   | 0111   |
| 8   | 1000   |
| 9   | 1001   |
| A   | 1010   |
| B   | 1011   |
| C   | 1100   |
| D   | 1101   |
| E   | 1110   |
| F   | 1111   |

**2. Hexadecimal to Binary**

* Replace each hex digit with its 4-bit binary equivalent.

**Example: Convert 0x3C to binary**

```
3 = 0011  
C = 1100

Binary: 00111100
```