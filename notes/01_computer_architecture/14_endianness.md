# Endianness

> **Random Quote:** Good code is its own best documentation.

**Endianness** refers to the sequence in which bytes are arranged when storing multi-byte data types in computer memory.

There are two primary types of byte order:

1. **Little-endian**
2. **Big-endian** (also referred to as **Network Byte Order**)

In **little-endian** systems, the **least significant byte (LSB)** is stored first. For example:

```
0x12345678 -> 78 56 34 12
```

In **big-endian** systems, the **most significant byte (MSB)** is stored first:

```
0x12345678 -> 12 34 56 78
```

---

## Where They Are Used

* **Intel x86/x86\_64** processors use **little-endian** format.
* **PowerPC, SPARC, and some ARM** systems use **big-endian** format.
* **ARM** architecture is **bi-endian**, meaning it can operate in either mode.
* **Network protocols** (such as TCP/IP) use **big-endian**, which is why it is commonly referred to as **network byte order**.

A computer uses the **host byte order**, which depends on its CPU architecture. For example, systems using Intel processors typically use little-endian, while others may use big-endian.

---

## Real-World Implications

* **Binary file parsing:** Correct interpretation of byte order is necessary when reading or writing binary files.
* **Network programming:** Programs must often convert between host and network byte orders to ensure data is transmitted and received correctly.
* **Cross-platform compatibility:** Systems with different endianness can encounter errors if byte order is not handled properly during data exchange.

---

## Detecting Endianness

The following C program demonstrates how to determine a system's endianness:

```c
#include <stdio.h>

int main() {
    unsigned int x = 1;
    char *c = (char*)&x;

    if (*c)
        printf("Little-endian\n");
    else
        printf("Big-endian\n");

    return 0;
}
```

### Explanation

* `unsigned int x = 1;` creates a 4-byte integer with the value `0x00000001`.
* `char *c = (char*)&x;` casts the address of `x` to a `char*`, pointing to the first byte of `x` in memory.
* The condition `if (*c)` examines the first byte:

  * If the result is non-zero (i.e., `0x01`), the system is **little-endian**.
  * If the result is zero (i.e., `0x00`), the system is **big-endian**.

---

## Converting Between Endianness

There are two common data sizes typically converted between endianness:

1. **short** (2 bytes)
2. **long** (4 bytes)

### Using Standard Functions

These conversions are performed using well-defined functions with the following naming pattern:

* `htons()` – Host to network short
* `htonl()` – Host to network long
* `ntohs()` – Network to host short
* `ntohl()` – Network to host long

These functions are part of standard networking libraries and ensure proper byte order conversion across systems.

### Manual Conversion Using Bitwise Operations

The following function performs a manual byte swap on a 32-bit unsigned integer:

```c
uint32_t swap32(uint32_t val) {
    return ((val >> 24) & 0x000000FF) |
           ((val >> 8)  & 0x0000FF00) |
           ((val << 8)  & 0x00FF0000) |
           ((val << 24) & 0xFF000000);
}
```

This function reverses the byte order. For instance, passing `0x12345678` will return `0x78563412`.

#### Breakdown

1. `(val >> 24) & 0x000000FF` isolates the most significant byte and shifts it to the least significant position.
2. `(val >> 8) & 0x0000FF00` extracts the second byte from the left.
3. `(val << 8) & 0x00FF0000` shifts the second byte from the right into its new position.
4. `(val << 24) & 0xFF000000` moves the least significant byte to the most significant position.
5. The bitwise OR operator (`|`) combines all four parts to form the reversed value.

---

## Endianness in Assembly

On **little-endian** systems, pushing the 32-bit value `0x12345678` onto the stack results in the following memory layout:

```
[SP+0] = 0x78
[SP+1] = 0x56
[SP+2] = 0x34
[SP+3] = 0x12
```

This knowledge is essential in contexts such as:

* Understanding system call ABI conventions
* Analyzing memory dumps and debugging
* Interpreting trap frames and register states