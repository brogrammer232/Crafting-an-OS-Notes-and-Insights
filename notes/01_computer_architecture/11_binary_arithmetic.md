> This file should be deleted soon. It contains information on number systems arithmetic. This information will be placed in the `08_number_systems.md` file once I figure out how I will place it to make the information flow and not seem all over the place.


# Binary Arithmetic
Spoiler alert! Binary math isn't hard. It's just unfamiliar. Until it's not. Then it's fun.

**Key Topics:**
+ [Addition](#addition)
+ [Subtraction](#subtraction)
+ [Overflow and Carry](#overflow-and-carry)

Binary arithmetic is essential, not just for calculations, but for bit manipulation, flag setting, addressing, permissions, and interrupt control. The CPU performs all math in binary, so understanding how it works at the bit level is crucial.

## Addition
Here are the rules for binary addition:

| A | B | A+B | Carry                        |
| - | - | --- | ---------------------------- |
| 0 | 0 | 0   | 0                            |
| 0 | 1 | 1   | 0                            |
| 1 | 0 | 1   | 0                            |
| 1 | 1 | 0   | 1   ← (1 + 1 = 10 in binary) |

Think of it like base-10: 1 + 1 = 2, and 2 in binary is `10`, so you write `0` and carry `1`.

Example:
```
    0110 (6)        1110 (14)
  + 0101 (5)      + 1101 (13)
  ----------      -----------
    1011 (11)      11011 (27)
```

## Subtraction
Here are the rules for binary subtraction:

| A | B | A−B | Borrow                                     |
| - | - | --- | ------------------------------------------ |
| 0 | 0 | 0   | 0                                          |
| 1 | 0 | 1   | 0                                          |
| 1 | 1 | 0   | 0                                          |
| 0 | 1 | 1   | 1    ← You had to borrow from the left bit |

The last one is confusing, isn't it. You are probably thinking that `A-B` should be 0 after borrowing 1. Me too. Well it's 1. Deal with it.

Example:
```
    0110 (6)        1100 (12)
  - 0011 (3)      - 1010 (10)
  ----------      -----------
    0011 (3)        0010 (2)
```

## Overflow and Carry
Binary operations in CPUs are fixed-width (usually 8, 16, 32, or 64 bits). This means that results that exceed the bit width cause overflow.

+ The **Carry Flag (CF)** is set when an **unsigned** addition/subtraction produces a carry/borrow out of the MSB.
+ The **Overflow Flag (OF)** is set when a **signed** addition/subtraction results in a sign change that shouldn't happen (e.g, positive + positive = negative).

**Example: Unsigned**
```
# Assume we are working with 4 unsigned bits.

    1000 (8)
  + 1001 (9)
  ----------
   10001 (17) # This exceeds 4 bits (overflow).
```
Since this is **unsigned**, you'll get `0001` (1), with the **carry flag set** because there was a carry out of the MSB.

**Example: Signed**
```
# Assume we are working with 4 signed bits.
    0100 (+4)
  + 0101 (+5)
  -----------
    1001 (-7) # Wrong. Correct answer is +9.
```
In this case, since this is **signed**, you'll get `1001` with the **overflow flag set** because the sign bit changed incorrectly and the result is too big to be represented correctly in signed 4-bit format.
