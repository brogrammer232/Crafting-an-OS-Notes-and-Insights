# Endianness

> **Random Quote:** Good code is its own best documentation.

Endianness defines the order in which bytes are stored for multi-byte data types in computer memory.

There are 2 types of byte orders:
1. **Little-Endian**
2. **Big-Endian / Network Byte Order**

In **little-endian**, the LSB (least significant byte) comes first. Example: `0x12345678` will be stored as `78 56 34 12`.

In **big-endian**, the MSB (most significant byte) comes first. Example: `0x12345678` will be stored as `12 34 56 78`.

---

### Where they are used
+ **Intel** CPUs use **little-endian**.
+ **PowerPC, SPARKC, some ARM systems** use **big-endian**.
+ **ARM** devices can run in **bi-endian** mode. This means that they can use both big-endian and little-endian.
+ The **network** uses **big-endian**. That's why it's also called **network byte order**. Internet protocols like TCP/IP use big-endian.

Your computer stores numbers in **host byte order**. If it's an intel, host byte order is little-endian. If it's a SPARKC (or something else that uses big-endian), host byte order is big-endian.

---

### Detecting Endianness
This C program determines whether the system is little-endian or big-endian by looking at how multi-byte value is stored in memory.

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

### Converting Between Endianness
There are 2 types of numbers that you can convert:
1. **short**: Two bytes.
2. **long**: Four bytes.

Remenber this:
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

| **Function** 	| **Description** 	|
|---------------|-------------------|
| `htons()` 	| Host to network short |
| `htonl()` 	| Host to network long 	|
| `ntohs()` 	| Network to host short |
| `ntohl()` 	| Network to host long 	|

#### Manual conversion
If you love pain, you can also use bitwise operations to do this manually.

**Example:**
```c
uint32_t swap32(uint32_t val) {
    return ((val >> 24) & 0x000000FF) |
           ((val >> 8)  & 0x0000FF00) |
           ((val << 8)  & 0x00FF0000) |
           ((val << 24) & 0xFF000000);
}
```

This function, `swap32()` reverses the byte order of a 32-bit unsigned integer. If you input big-endian, it will output little-endian and vice-versa. This is called **byte swapping**, and it manually flips the byte order using bitwise operations.
