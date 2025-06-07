# Endianness

> **Random Quote:** Good code is its own best documentation.

Endianness defines the order in which bytes are stored for multi-byte data types in computer memory.

There are 2 main types of byte orders:

1. **Little-endian**
2. **Big-endian** (also called **Network Byte Order**)

In **little-endian**, the **least significant byte (LSB)** comes first. Example:

```
0x12345678 -> 78 56 34 12
```

In **big-endian**, the **most significant byte (MSB)** comes first. Example:

```
0x12345678 -> 12 34 56 78
```

---

## Where They're Used

* **Intel x86/x86\_64** CPUs use **little-endian**.
* **PowerPC, SPARC, some ARM** systems use **big-endian**.
* **ARM** is **bi-endian**: can operate in both modes.
* The **network** (e.g. TCP/IP) uses **big-endian**, which is why it's called **network byte order**.

Your computer stores numbers in **host byte order**. If it's an intel, host byte order is little-endian. If it's a SPARKC (or something else that uses big-endian), host byte order is big-endian.

### Real-World Implications

* **Binary file parsing:** You must know the byte order used in file storage.
* **Network programming:** You often need to convert between host and network byte order.
* **Cross-platform bugs:** Data sharing between different-endian systems can break things if not handled.

---

## Detecting Endianness
This program checks how a multi-byte integer is stored in memory:

```C
#include <stdio.h>

int main() {
    unsigned int x = 1;
    char *c = (char*)&x;
    
    if (*c) printf("Little-endian\n");
    else    printf("Big-endian\n");

    return 0;
}
```

**Explanation:**

+ `unsigned int x = 1;`: This line declares a 4-byte (typically) integer with the value `1`. It looks like this in hexadecimal: `0x00000001`.

+ `char *c = (char *)&x;`: This takes the address of the integer `x`, casts it to a character pointer (`char*`), and assigns it to `c`. Note:
    - `unsigned int` is 4 bytes.
    - `char` is 1 byte.
    - This allows us to examine the integer, byte by byte.
    - At this stage, `c` points to the first byte of `x` in memory.

+ The `if` statement:
    ```c
    if (*c) printf("Little-endian\n");
    else    printf("Big-endian\n");
    ```

    + This looks at the first byte of `x` as stored in memory. As mentioned earlier, `c` points to that first byte.
    + The value of `*c` depends on how the integer was stored in memory. 
        - If big-endian: `00 00 00 01`. `*c = 0x00`.
        - If little-endian: `01 00 00 00`. `*c = 0x01`.

    + The `if` statement returns true if the value is not zero (`0x01`. This prints out `"Little-endian"`.
    + Else, if the value is 0 (`0x00`), the program prints out `"Big-endian"`. Cool.

---

## Converting Between Endianness
There are 2 types of numbers that you can convert:

1. **short**: Two bytes.
2. **long**: Four bytes.

### Using Standard Functions

Mnemonic:

+ Host: `h`
+ Network: `n`
+ Short: `s`
+ Long: `l`

Say you want to convert a short number from host byte order to network byte order. Here's how you get the name of the function to use.
+ Start with the host: `h`
+ Follow it with: `to`
+ Then the network: `n`
+ Finally its size, short: `s`.
+ The function name is: `htons()`.
+ This is read as: "Host to network short."

You can use the other combinations to come up with the following functions:

| Function  | Meaning               |
| --------- | --------------------- |
| `htons()` | Host to network short |
| `htonl()` | Host to network long  |
| `ntohs()` | Network to host short |
| `ntohl()` | Network to host long  |

#### Manual conversion
If you love pain, you can do it manually using bitwise operations.

```c
uint32_t swap32(uint32_t val) {
    return ((val >> 24) & 0x000000FF) |
           ((val >> 8)  & 0x0000FF00) |
           ((val << 8)  & 0x00FF0000) |
           ((val << 24) & 0xFF000000);
}
```

This function, `swap32()` reverses the byte order of a 32-bit unsigned integer. If you input big-endian, it will output little-endian and vice-versa. This is called **byte swapping**, and it manually flips the byte order using bitwise operations.

**Breakdown:**
+ Let's assume we pass in `0x12345678`. The function should reverse it and return `0x78563412`.

+ **Step 1:** Move the top byte `0x12`, all the way to the left.
    - `((val >> 24) & 0x000000FF)`.
    - Note that val in our case is `0x12345678`.
    - `(0x12345678 >> 24)` = `0x00000012`.
    - `0x00000012 & 0x000000FF` = `0x00000012`. This makes sure that all the other bytes are zero.

+ **Step 2:** `((val >> 8) & 0x0000FF00)`
    - `(0x12345678 >> 8)` = `0x00123456`
    - `0x00123456 & 0x0000FF00` = `0x00003400`

+ **Step 3:** `((val << 8) & 0x00FF0000)`
    - `(0x12345678 << 8)` = `0x34567800`
    - `0x34567800 & 0x00FF0000` = `0x00560000`

+ **Step 4:** `((val << 24) & 0xFF000000)`
    - `(0x12345678 << 24)` = `0x78000000`
    - `0x78000000 & 0xFF000000` = `0x78000000`

+ **Step 5:** Put it all together.
    ```
    0x00000012 | 0x00003400 | 0x00560000 | 0x78000000
    Final result: 0x78563412
    ```
    Cool. Reverse successfull!

+ Bitwise `AND` masks out unwanted bits after shifting.
+ Bitwise `OR` combines the shifted pieces.

---

## Endianness in Assembly

On a **little-endian** machine, pushing `0x12345678` onto the stack results in:

```
[SP+0] = 0x78
[SP+1] = 0x56
[SP+2] = 0x34
[SP+3] = 0x12
```

Important for understanding:

* System call ABI expectations
* Memory dumps and debugging
* Traps and register state layouts

---