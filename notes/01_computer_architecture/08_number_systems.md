# Number Systems

> **Random Quote:** You don't have to see the whole system call, just the next instruction.

A **number system** is a way to represent and interpret numeric values. In OS development, you mainly use:
+ [Decimal](#decimal)
+ [Binary](#binary)
+ [Hexadecimal](#hexadecimal)

It's crucial you know how to convert from one number system to the other:
+ [Decimal and Binary](#decimal-and-binary)
+ [Decimal and Hexadecimal](#decimal-and-hexadecimal)
+ [Binary and Hexadecimal](#binary-and-hexadecimal)

## Decimal
The decimal system is the standard numerical base used in daily human activity, e.g, 1, 3 35.

**Base:** Decimal operates on **base 10**, meaning each digit represents a power of 10 depending on its position. To make it simpler, a number in decimal is the sum of each digit multiplied by 10 raised to the digit's position (right to left, base 0). Example:
```
745 = (7 x 10^2) + (4 x 10^1) + (5 x 10^0) = 700 + 40 + 5
82 = (8 x 10^1) + (2 x 10^0) = 80 + 2
```

**Use:** Decimal is mostly used for display purposes or user input. Internally, computers do not operate in decimal at all, it's translated for human convenience.

**Writing in programs:** Decimal is written as is. Example:
```assembly
INC AX, 8   ;8 is a decimal number.
```

## Binary
The binary system is the native language of digital computers. It uses only two symbols: `0` and `1`. It represents: on/off, true/false, or low/high voltage states.

**Base:** Binary is **base 2**, meaning, each bit (binary digit) represents a power of 2. Positions go from right to left. Example:
```
100 = (1 x 2^2) (0 x 2^1) (0 x 2^0)
```

**Use:** The CPU does everything in binary: all arithmetic, bit patterns are directly mapped to instructions, flags, registers, GDT entry values, etc. Example:
```
"10111000 00000001 00000000 00000000 00000000" means "MOV EAX, 0x1".
```

**Writing in programs:** When writing binary in programs, prefix it with `0b`. Example:
```assembly
MOV EAX, 0b10110001 ;Here, the binary is "10110001".
```

## Hexadecimal
Hexadecimal is a compact, human-friendly representation of binary. It uses 16 digits: `0-9` and `A-F`(10-15). Numbers 10-15 are represented by letters:
+ A = 10
+ B = 11
+ C = 12
+ D = 13
+ E = 14
+ F = 15

**Base:** Hexadecimal is **base 16**, meaning each digit is a power of 16 depending on its position. Example:
```
2F = (2 x 16^1) (15 x 16^0) # F=15.
```

**Use:** Hexadecimal is used in a lot of places: all memory location are shown in hex, tools like `hexdump`, `objdump` display binary data in hex, dissassemblers show hex instructions, register values are usually shown in hex, among others. It's easier to write hex compared to writing binary.

**Writing in programs:** When writing hexadecimal, prefix it with `0x`. Example:
```assembly
MOV AX, 0xB8000 ;Something fun, 0xB8000 is the start of VGA text buffer. You'll enjoy playing with this.
```
In some assemblers, you can suffix it with `h`. Example:
```assembly
MOV EAX, B8000h ;Same as the above.
```

## Conversions
### Decimal and Binary
1. **Decimal to Binary:**
    + Divide the number by 2 repeatedly until you get to 0.
    + Write down the remainder each time.
    + The binary number is the remainders read from bottom to top.
    
    **Example:** Convert `13` to binary.
    ```
    13 / 2 = 6 remainder 1
     6 / 2 = 3 remainder 0
     3 / 2 = 1 remainder 1
     1 / 2 = 0 remainder 1

    The binary form of 13 is 1101.
    ```

2. **Binary to Decimal:**
    + Multiply each bit with `2^n` where `n` is its position from the right.
    + Add the results.

    **Example:** Convert `1101` to decimal.
    ```
    1101 = (1 x 2^3) + (1 x 2^2) + (0 x 2^1) + (1 x 2^0) = ...
    ... = 8 + 4 + 0 + 1 = 13

    The decimal form of 1101 is 13.
    ```

### Decimal and Hexadecimal
1. **Decimal to Hexadecimal:**
    + Divide the number by 16 repeatedly until you get to 0.
    + Record remainders (in hex notation).
    + Read from bottom up.

    **Example:** Convert `430` to hex.
    ```
    430 / 16 = 26 remainder 14 (E).
     26 / 16 = 1  remainder 10 (A).
     1 / 16  = 0 remainder 1 (1).

    The hexadecimal form of 430 is 0x1AE.
    ```

2. **Hexadecimal to Decimal:**
    + Multiply each digit by `16^n` where `n` is its position.
    + Add the results.

    **Example:** Convert `0x1AE` to decimal.
    ```
    0x1AE = (1 x 16^2) + (10 x 16^1) + (14 x 16^0) = ...
    ... = 256 + 160 + 14 = 430

    The decimal form of 0x1AE is 430.
    ```

### Binary and Hexadecimal
1. **Binary to Hexadecimal:**
    + Group bits into *nibbles* (4-bit groups) from right to left.
    + Convert each nibble to hex.

    **Example:** Convert `0b10111110` to hexadecimal.
    ```
    10111110 = 1011 and 1110
    1011 = B, 1110 = E

    The hexadecimal form of 0b10111110 is 0xBE.
    ```

    How did I know that `1011` is B and `1110` is E. You can do this in two ways:
        1. Convert the binary to decimal then the decimal to hex.
        2. Use a table that shows the binary alongside the hex value.
    Personally, I recommend converting binary -> decimal -> hexadecimal. You won't always have the table, and honestly, I think it's faster to convert compared to looking for the file containing the table or having to google it (you may not always have an internet connection). Here's the table is you prefer it:
   
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

2. **Hexadecimal to Binary:**
    + Replace each hex digit with its 4-bit binary equivalent. You can check the table or convert hexadecimal -> decimal -> binary. Your call.

    **Example:** Convert `0x3C` to binary.
    ```
    3 = 0011
    C = 1100

    The binary form of 0x3C is 00111100.
    ```

