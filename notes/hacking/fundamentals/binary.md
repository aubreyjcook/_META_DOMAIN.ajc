# Binary

## What Binary Is

Binary is the base-2 number system. It uses exactly two symbols — `0` and `1` — which is why a single binary digit is called a **bit** (a contraction of "binary digit").

By contrast, the decimal system we count in is base-10: ten symbols (`0`–`9`). Any number expressible in decimal is expressible in binary; only the notation changes, not the value.

## Why Computers Use Binary

A computer stores and moves information as physical states that are easy to distinguish reliably. Two states — on/off, high/low voltage, charge/no charge — are far easier to detect without error than ten distinct voltage levels. A transistor, the fundamental switch of modern processors, is naturally a two-state device: it conducts or it does not.

So at the hardware level, *everything* — numbers, text, images, programs, addresses — is represented as patterns of `0`s and `1`s.

## Bits and Bytes

- **Bit** — one binary digit (`0` or `1`).
- **Nibble** — 4 bits.
- **Byte** — 8 bits; the smallest unit most hardware addresses directly.
- **Word** — a larger group (16, 32, or 64 bits) that the CPU processes at once; the "word size" is what makes a processor 32-bit or 64-bit.

## Place Values

Binary is a positional system, just like decimal. In decimal each position is a power of 10 (ones, tens, hundreds); in binary each position is a power of 2.

For an 8-bit value, the place values are:

| Bit position | 7 | 6 | 5 | 4 | 3 | 2 | 1 | 0 |
|---|---|---|---|---|---|---|---|---|
| Power of 2   | 2⁷ | 2⁶ | 2⁵ | 2⁴ | 2³ | 2² | 2¹ | 2⁰ |
| Value        | 128 | 64 | 32 | 16 | 8 | 4 | 2 | 1 |

A `1` in a position adds that position's value; a `0` adds nothing.

**Example:** `00001011` = 8 + 2 + 1 = **11**.

The leftmost bit is the **most significant bit (MSB)**; the rightmost is the **least significant bit (LSB)**.

## Converting Binary to Decimal

Add the place values of every position holding a `1`.

```
101101
= 1×32 + 0×16 + 1×8 + 1×4 + 0×2 + 1×1
= 32 + 8 + 4 + 1
= 45
```

## Converting Decimal to Binary

**Method 1 — repeated division by 2.** Divide the number by 2, record the remainder (`0` or `1`), and repeat with the quotient until it reaches 0. Read the remainders from bottom to top.

```
Convert 45:
45 ÷ 2 = 22 remainder 1
22 ÷ 2 = 11 remainder 0
11 ÷ 2 =  5 remainder 1
 5 ÷ 2 =  2 remainder 1
 2 ÷ 2 =  1 remainder 0
 1 ÷ 2 =  0 remainder 1   <- stop

Read bottom-to-top: 101101
```

**Method 2 — subtract the largest power of 2.** Find the largest power of 2 that fits, mark a `1`, subtract, and continue down the powers.

```
Convert 45:
45 - 32 = 13   -> 1 (32)
13 - 16 < 0    -> 0 (16)
13 -  8 =  5   -> 1 (8)
 5 -  4 =  1   -> 1 (4)
 1 -  2 < 0    -> 0 (2)
 1 -  1 =  0   -> 1 (1)

Result: 101101
```

## Counting in Binary

| Decimal | Binary (4-bit) | Hex |
|--------:|:--------------:|:---:|
| 0  | 0000 | 0 |
| 1  | 0001 | 1 |
| 2  | 0010 | 2 |
| 3  | 0011 | 3 |
| 4  | 0100 | 4 |
| 5  | 0101 | 5 |
| 6  | 0110 | 6 |
| 7  | 0111 | 7 |
| 8  | 1000 | 8 |
| 9  | 1001 | 9 |
| 10 | 1010 | A |
| 11 | 1011 | B |
| 12 | 1100 | C |
| 13 | 1101 | D |
| 14 | 1110 | E |
| 15 | 1111 | F |

## Binary and Hexadecimal

Notice the right column above: four binary digits always map to exactly one hexadecimal digit. Because `2⁴ = 16`, a nibble (4 bits) is one hex digit, and two hex digits are one byte. That is why [hexadecimal](hexadecimal.md) is the standard shorthand for reading binary.

## Why This Matters for Hacking

- **Data representation** — every file, string, and value is ultimately a binary pattern; understanding that is the basis for understanding how programs store and interpret data.
- **Bit flags and bitmasks** — permissions (e.g. `chmod 755`), status registers, and packed options are individual bits with meaning.
- **Addresses and sizes** — memory addresses, register widths, and subnet masks are all understood in terms of bits and powers of 2.

## See Also

- [Hexadecimal](hexadecimal.md)
- [Numeric PIN ciphering](../numeric-pin-ciphering.md) — combinations of a code are `base^length`; for binary that is `2ⁿ`.
