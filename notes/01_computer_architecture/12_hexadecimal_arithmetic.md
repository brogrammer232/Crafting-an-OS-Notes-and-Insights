# Hexadecimal Arithmetic

> **Random Quote:** Simplicity is the soul of efficiency.

Hexadecimal is very similar to decimal arithmetic. The main difference is that the digits go from `0 - F`. This means that you carry/borrow at base 16 instead of 10 (this will make sense soon).

You can convert to decimal to simplify intermediate steps or avoid hexadecimal arithmetic entirely. Make sure you convert back to hexadecimal after.

**Key Topics:**
+ [Addition](#addition)
+ [Subtraction](#subtraction)
+ [Multiplication](#multiplication)
+ [Division](#division)

## Addition
**Example:** Add `0x3A` and `0x4F`.

```
    3A
  + 4F
  ----
    89
```

Add starting from the right.
+ A(10) + F(15) = 25
    - 25 in hex is 9, carry 1. This surprised me too.
    - To convert 25 to hexadecimal, divide it by 16 (since hex is base-16).

    ```
    25 / 16 = 1 remainder 9
    ```
    - So we write 9 and carry the 1 into the next digit.

+ 3 + 4 + 1(carry) = 8

This is how the final answer comes to 89. `0x3A + 0x4F = 0x89`.

**Simpler method:** Convert to decimal.
```
 3A = 58
 4F = 79

 58 + 79 = 137
 137 = 0x89
```

In case you have forgotten how to convert between hexadecimal and binary, check out [this file](./08_number_systems.md)

## Subtraction
Use borrow when needed, base-16 style.

**Example:** `0x4F` - `0x2A`
```
    4F
  - 2A
  ----
    25
```

+ F(15) - A(10) = 5
+ 4 - 2 = 2

The result is `0x25`.

**Example with borrow:** `0xF1` - `0x28`
```
    F1
  - 28
  ----
    C9
```

+ `1 - 8` is *"not possible"*. Therefore, we borrow 1 from F. F is left as E and 1 becomes `16 + 1` (remember, base-16).
    - After the borrow: `17 - 8 = 9`.

+ E(14) (remember we borrowed 1) - 2 = 12 (C).

The result is `0xC9`. How cool is that.

**Confusing example:** `0xF1` - `0x22`
```
    F1
  - 22
  ----
    CF
```

+ `1 - 2` is *"not possible"*. Therefore, we borrow 1 from F. F is left as E and 1 becomes `16 + 1` (base-16).
    - After borrowing: `17 - 2 = 15`.
    - I got confused a bit and thought we should write 5 and carry 1. Remember this is base-16. Carry starts at 16. 15 is F.

+ E(14) - 2 = 12 (C)

## Multiplication
**Example:** `0xA` multiplied by `0x5`
+ A(10) x 5 = 50
+ 50 in hex is `0x32`.

`0xA * 0x5 = 0x32`.

## Division
**Example:** `0x64` divide by `0x4`
+ `0x64` = 100
+ `0x4` = 4
+ 100 / 4 = 25
+ 25 = `0x19`

`0x64 / 0x4 = 0x19`.

## Final Thoughts
Hexadecimal math might sound scary, but once you treat it like base-10 with fancier digits, it all clicks. Just remember that carries happen at 16, not 10.
