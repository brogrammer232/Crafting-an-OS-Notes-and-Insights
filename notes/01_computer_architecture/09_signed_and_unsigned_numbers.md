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

### Unsigned Binary
Definition

How it works: with example

Why it's used/not used

Where it's used

### Sign-magnitude
Sign-magnitude is a straight forward, easy to understand binary format.

**How it works:**
+ The MSB (most significant bit) indicates the sign (`0` means positive, `1` means negative).
+ The remaining bits represent the magnitude (absolute value).

Example:
+ `+3` = `00000011`
+ `-3` = `10000011`

**Why it's not used:**
+ You need special hardware logic for signed arithmetic using sign-magnitude.
+ There are two versions of 0:
    - `00000000` = +0
    - `10000000` = -0
    This causes bugs and makes comparisons messy.
+ Addition and subtraction require separate rules for signs.

Sign-magnitude is easy for humans to read, but bad for CPUs.

### One's complement
This is very similar to two's complement.

**How it works:**
+ Positive numbers are the same as unsigned.
+ Negative numbers are stored by inverting all the bits. We don't add 1 like in two's complement.

Example:
+ `+5`: `00000101`
+ `-5`: `11111010` (Inverted the bits)

**Why it's not used:**
This was completely replaced by two's complement. Here's why:
+ Has two zeroes:
    - `00000000` = +0
    - `11111111` = -0
    As mentioned earlier, this causes bugs and makes comparisons messy.
+ Arithmetic is harder because you must handle carries manually.

You can safely forget everything about this now. It won't come back to haunt you. Or will it? ...

### Two's complement
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

**Why it's used:**
+ Makes addition/subtraction simple and fast. Same logic for both positive and negative numbers.
+ Only one representation of 0, unlike other schemes (like sign-magnitude).
+ Overflow wraps around naturally.

Two's complement is wierd for humans, but perfect for machines.

**Where it's used:**
+ All modern processors use two's complement for signed integers.
+ Compilers assume two's complement when doing signed arithmetic.
+ Operating systems use it too in kernel code, system calls, memory management, drivers, you name it.
+ Binay protocols and file formats that store signed number often assume two's complement.
+ Embedded systems and microcontrollers, stuff like arduino, use two's complement for signed math.

The list goes on. Two's complement is the industry standard. Everywhere signed integers are used in hardware or low-level code, it's two's complement.

### Excess-K
This is also know as biased representation.

**How it works:**
+ You add a bias (K) to the actual number so everything becomes non-negative.
+ Stored value = Actual value + bias (K)

Example: Excess-127 (used in IEEE 754 32-bit floats)
+ Bias = 127
+ `-2` = `-2 + 127 = 125` = `01111101`
+ `0` = `0 + 127 = 127` = `01111111`


This makes sorting and comparison easy. All exponents become unsigned, clean and fast in hardware.

**Why it's used:**
+ It makes comparison easy.
+ Simplifies floating-point parsing.
+ Non-negative representation.

**Where it's used:**
+ IEEE 754 fload/dobule formats
+ CPU FPU registers
+ Emulators and compiler backends
