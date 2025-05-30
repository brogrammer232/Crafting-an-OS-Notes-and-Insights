# Signed and Unsigned Numbers

> **Random Quote:** The computer is incredibly fast, accurate, and stupid. Man is unbelievably slow, inaccurate, and brilliant. The marriange of the two is a force beyond calculation.

### Definition
**Unsigned numbers** can only represent **non-negative** values (zero and positive).

**Signed numbers** can represent **both positive and negative** values.

### Bit representation
Unsigned numbers use all bits for magnitude.

A signed number reserves one bit (usually the most significant bit, MSB) to indicate the sign.
+ `0`: Positive
+ `1`: Negative

This means that in a byte (8 bits), an unsigned number uses all the 8 bits to represent the number and a signed number uses only 7 bits. This leads to unsigned numbers being able to represent greater values using the same number of bits.

| Bits | Signed Range      | Unsigned Range |
| ---- | ----------------- | -------------- |
| 8    | -128 to 127       | 0 to 255       |
| 16   | -32,768 to 32,767 | 0 to 65,535    |
| 32   | \~ -2.1B to +2.1B | 0 to \~4.2B    |

# Integer Representations in Binary
Integer representations or signed number representations define how signed integers are encoded in binary.

The different ways of representing integers are:
+ **Unsigned binary**
+ **Sign-magnitude**
+ **One's complement**
+ **Two's complement**
+ **Excess-K**

You will only work with **unsigned binary** and **two's complement** for signed integers for the most part. Two's complement is the only signed integer representation used in: modern CPUs, operating systems, compilers, assembly languages, etc.

Knowing about sign-magnitude and one's complement helps you read old documentation, and gives you insight into why two's complement is better. Feel free to skip them.

Excess-K is still used in IEEE 754 floating point. It is worth knowing if you'll work with floating point hardware or emulation.

## Unsigned Binary

## Sign-magnitude

## One's complement

## Two's complement
Two's complement is the most common way computers represent signed integers in binary.

**How it works:**
+ Positive numbers are stored normally (just like unsigned).
+ Negative numbers are stored by inverting the bits and adding 1 to the result.

Example:
+ Positive 5 = `00000101`.
+ Negative 5:
    - Invert the bits: `11111010`.
    - Add 1: `11111010 + 1 = 11111011`.
    - -5 = `11111011` in two's complement.

Arithmetic example:
```
 5 = 00000101
-3 = 11111101 (in two's complement)

Adding them:
    00000101 (5)
  + 11111101 (-3)
  ---------------
   100000010

Ignore the overflow. The result is: 00000010 = 2
```

Why did we ignore the overflow? In two's complement, the overflow doesn't matter, it's not part of the result. Note that:
+ the result wraps around naturally and
+ the final bits already encode the correct signed result.
This is one of the reasons why two's complement is preferred over the other integer representations, because it's easy to do math with. It just works.

**The rule:** In two's complement, ignore the carry out. It's not part of the result. Use sign bit mismatch to detect signed overflow.

**Why two's complement is preferred:**
+ Makes addition/subtraction simple and fast. Same logic for both positive and negative numbers.
+ Only one representation of 0, unlike other schemes (like sign-magnitude).
+ Overflow wraps around naturally.

## Excess-K
