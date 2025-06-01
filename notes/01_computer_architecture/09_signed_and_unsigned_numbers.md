# Signed and Unsigned Numbers

> **Random Quote:** The computer is incredibly fast, accurate, and stupid. Man is unbelievably slow, inaccurate, and brilliant. The marriange of the two is a force beyond calculation.


## Unsigned Numbers
An unsigned numbers is a binary number that can only represent zero and positive values. All bits contribute to magnitude. There is no sign bit like in signed integers.

**Range:**

| Bits | Min | Max                        |
| ---- | --- | -------------------------- |
| 8    | 0   | 255                        |
| 16   | 0   | 65,535                     |
| 32   | 0   | 4,294,967,295              |
| 64   | 0   | 18,446,744,073,709,551,615 |

**Binary representation:**
This is straight forward.
+ `0` = `00000000`
+ `1` = `00000001`
+ `255` = `11111111`

**Overflow:**
Overflow occurs when a calculation produces a result outside the range representable by the signed type. If you go over the max, it wraps around.
Example: If you add 1 to 255 in 8-bit, the value will be 0.
```c
uint8_t = 255;
x += 1; // x = 0
```

**Why use unsigned numbers:**
+ You are sure that the value can't be negative (e.g., size of an array, memory address, etc).
+ They give you a wider positive range than signed types of the same size.
+ In some hardware-level programming, unsigned is the natural format.

**Things to watch out for:**
+ Comparing `unsigned` with `signed` values can lead to bugs:
    ```c
    int a = -1;
    unsigned int b = 1;

    (a < b) // FALSE? The MSF being 1, makes `a`'s unsigned version greater than `b`.
    ```

+ Subtraction can wrap if you're not careful.
    ```c
    unsigned int x = 5 - 10; // x = 4294967291 if 32-bit
    ```

## Signed Numbers
A signed number is a binary number that can represent both positive and negative values. The MSB (most significant bit) is used to indicate the sign and the other bits are used for the magnitude.
+ `0`: Positive
+ `1`: Negative

This means that in a byte (8 bits), 7 bits are used to represent the number and the MSB is used to represent the sign.

**Range:**

| Bits | Min            | Max            |
| ---- | -------------- | -------------- |
| 8    | -128           | +127           |
| 16   | -32,768        | +32,767        |
| 32   | -2,147,483,648 | +2,147,483,647 |


**Binary representation:**
There are multiple ways of representing signed numbers in binary. These different ways are called **integer representations** or **signed number representations** and they are:
+ [Two's complement](#twos-complement)
+ [One's complement](#ones-complement)
+ [Sign-magnitude](#sign-magnitude)
+ [Excess-K](#excess-k)

These are discussed below.

**Overflow:**
Overflow occurs when a calculation produces a result outside the range representable by the signed type. Like in unsigned numbers, signed wraps around.
```c
int8_t x = 127;
x += 1;     // Wraps to -128
```

The binary:
```
127 = 01111111
+1  = 00000001
     ---------
     10000000 = -128 (!!!)
```

Same for negative overflow:
```c
int8_t = -128;
x -= 1;     // wraps to +127
```

**Why use signed numbers:**
+ When you need to work with both negative and positive values or even negative only.

**Things to watch out for:**
+ Comparing `unsigned` with `signed` values can lead to bugs:
    ```c
    int a = -1;
    unsigned int b = 1;

    (a < b) // FALSE? The MSF being 1, makes `a`'s unsigned version greater than `b`.
    ```
+ Watch out for overflows and wrap arounds.

+ Never assume overflows wraps cleanly for signed integers. Signed integer overflow is undefined behavior in C and C++. This means that the compiler is allowed to assume it never happens. It may optimize your code in ways that make bugs invisible or unpredictable. This is not true for unsigned. Unsigned overflow is well-defined and just wraps around.

+ Be consistent with types. Don't mix signed and unsigned unless you're absolutely sure.

## Signed Number Representation
### Two's Complement

---

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

### One's Complement

---

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

### Sign-magnitude

---

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

### Excess-K

---

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
