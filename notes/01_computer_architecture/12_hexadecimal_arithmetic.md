# Hexadecimal Arithmetic

> **Random Quote:** Simplicity is the soul of efficiency.

Hexadecimal arithmetic works much like decimal arithmetic. The main difference is that hex digits go from `0` to `F` (0 to 15 in decimal). This means you carry or borrow at base 16 instead of 10. If that doesn't make sense now, it will in a minute.

You can always convert to decimal to simplify intermediate steps. Just make sure to convert back to hexadecimal afterward.

**Key Topics:**

* [Addition](#addition)
* [Subtraction](#subtraction)
* [Multiplication](#multiplication)
* [Division](#division)

---

## Addition

**Example:** Add `0x3A` and `0x4F`

```
    3A
  + 4F
  ----
    89
```

Work from right to left:

* `A` (10) + `F` (15) = 25

  * 25 in hex is `0x19`
  * So, write down 9 and carry 1

* `3` + `4` + `1` (carry) = 8

Final answer: `0x89`

**Shortcut method:** Convert to decimal.

```
0x3A = 58
0x4F = 79

58 + 79 = 137
137 = 0x89
```

Check [this file](./08_number_systems.md) if you forgot how to convert between hex, binary, and decimal.

---

## Subtraction

Just like decimal, but carry/borrow works in base 16.

**Example:** `0x4F` - `0x2A`

```
    4F
  - 2A
  ----
    25
```

* `F` (15) - `A` (10) = 5
* `4` - `2` = 2

Result: `0x25`

---

**Example with borrow:** `0xF1` - `0x28`

```
    F1
  - 28
  ----
    C9
```

* `1 - 8` isn't possible, so borrow 1 from `F`

  * `F` becomes `E`
  * `1` becomes `1 + 16 = 17`
  * `17 - 8 = 9`

* `E` (14) - `2` = 12 (`C`)

Result: `0xC9`

---

**Trickier one:** `0xF1` - `0x22`

```
    F1
  - 22
  ----
    CF
```

* `1 - 2` needs a borrow

  * Borrow from `F`, now `E`
  * `1` becomes `17`
  * `17 - 2 = 15` → `F`

* `E - 2 = C`

Result: `0xCF`

---

## Multiplication

**Example:** `0xA * 0x5`

* `A` = 10
* `10 * 5 = 50`
* `50 = 0x32`

Result: `0x32`

---

## Division

**Example:** `0x64 / 0x4`

* `0x64` = 100
* `0x4` = 4
* `100 / 4 = 25`
* `25 = 0x19`

Result: `0x19`

---

## Final Thoughts

Hex math isn't black magic. Carries and borrows happen at 16, not 10. That’s the only real twist. Once you’re used to it, working with memory addresses, color values, or instruction encodings becomes second nature.