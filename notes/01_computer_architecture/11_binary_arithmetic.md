# Binary Arithmetic

> **Random Quote:** In theory, there is no difference between theory and practice. In practice, there is.

Spoiler alert: Binary math isn’t hard. It’s just unfamiliar. Until it’s not. Then it’s fun.

**Key Topics:**

* [Addition](#addition)
* [Subtraction](#subtraction)
* [Overflow and Carry](#overflow-and-carry)

Binary arithmetic is essential not just for calculations, but for bit manipulation, flag setting, addressing, permissions, and interrupt control. The CPU does all math in binary, so understanding it at the bit level is crucial.

---

## Addition

Here are the rules for binary addition:

| A | B | A + B | Carry |
| - | - | ----- | ----- |
| 0 | 0 | 0     | 0     |
| 0 | 1 | 1     | 0     |
| 1 | 0 | 1     | 0     |
| 1 | 1 | 0     | 1     |

Think of it like base 10: `1 + 1 = 2`, and `2` in binary is `10`, so you write `0` and carry `1`.

**Examples:**

```
    0110 (6)        1110 (14)
  + 0101 (5)      + 1101 (13)
  ----------      -----------
    1011 (11)       11011 (27)
```

---

## Subtraction

Here are the rules for binary subtraction:

| A | B | A - B | Borrow |
| - | - | ----- | ------ |
| 0 | 0 | 0     | 0      |
| 1 | 0 | 1     | 0      |
| 1 | 1 | 0     | 0      |
| 0 | 1 | 1     | 1      |

The last one might confuse you. You’re probably thinking `A - B` should be `0` after borrowing. Me too. But it’s `1`. Deal with it.

**Examples:**

```
    0110 (6)        1100 (12)
  - 0011 (3)      - 1010 (10)
  ----------      -----------
    0011 (3)         0010 (2)
```

---

## Overflow and Carry

Binary operations in CPUs are fixed-width (usually 8, 16, 32, or 64 bits). So when results exceed the bit width, you get an overflow.

* The **Carry Flag (CF)** is used in **unsigned** arithmetic. It is set when there's a carry out (in addition) or a borrow (in subtraction) from the most significant bit.
* The **Overflow Flag (OF)** is used in **signed** arithmetic. It is set when the result of an operation flips the sign incorrectly.

---

**Example: Unsigned Overflow**

```
# Using 4-bit unsigned numbers:

    1000 (8)
  + 1001 (9)
  ----------
   10001 (17)  --> Overflow

# Result is 0001 (1) with Carry Flag = 1
```

**Example: Signed Overflow**

```
# Using 4-bit signed numbers:

    0100 (+4)
  + 0101 (+5)
  -----------
    1001 (-7)  --> Wrong

# Correct answer should be +9, but 4-bit signed max is +7.
# Result is 1001 with Overflow Flag = 1
```