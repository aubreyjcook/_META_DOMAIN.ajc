# Hexadecimal

## What Hexadecimal Is

Hexadecimal ("hex") is the base-16 number system. It uses sixteen symbols: the digits `0`–`9` followed by the letters `A`–`F`, where `A` = 10 through `F` = 15.

## Why Hexadecimal Exists

Binary is what the machine uses, but it is unreadable to humans — a single byte is eight digits, and a 32-bit address is 32 digits. Hex compresses binary exactly, with no loss and no rounding, because **one hex digit represents exactly four bits (a nibble), and two hex digits represent exactly one byte.**

| Binary (4-bit) | Hex |
|:--------------:|:---:|
| 0000 | 0 |
| 0001 | 1 |
| 0010 | 2 |
| 0011 | 3 |
| 0100 | 4 |
| 0101 | 5 |
| 0110 | 6 |
| 0111 | 7 |
| 1000 | 8 |
| 1001 | 9 |
| 1010 | A |
| 1011 | B |
| 1100 | C |
| 1101 | D |
| 1110 | E |
| 1111 | F |

## Hex Digits and Place Values

Hex is positional like decimal and binary, but each position is a power of 16: `… 16³ 16² 16¹ 16⁰` (= 4096, 256, 16, 1).

**Example:** `1A3F`

```
1×16³ + A×16² + 3×16¹ + F×16⁰
= 1×4096 + 10×256 + 3×16 + 15×1
= 4096 + 2560 + 48 + 15
= 6719
```

## Converting Between Hex and Binary

This is the easy direction — no arithmetic required, just grouping.

**Hex → binary:** expand each hex digit into its 4-bit equivalent.

```
A3 -> A (1010) and 3 (0011) -> 10100011
```

**Binary → hex:** pad the binary to a multiple of 4 bits, then group into nibbles.

```
10100011 -> 1010 0011 -> A 3 -> A3
```

## Converting Between Hex and Decimal

**Hex → decimal:** sum each digit times its place value (the `1A3F` example above).

**Decimal → hex:** repeatedly divide by 16 and record remainders (the same method as decimal-to-binary), or convert decimal → binary first and then group into nibbles.

```
Convert 255:
255 ÷ 16 = 15 remainder 15  -> F
 15 ÷ 16 =  0 remainder 15  -> F

Read bottom-to-top: FF
```

`FF` = 255, the maximum value of a single byte — a number that appears constantly in networking, color, and storage limits.

## Common Prefixes and Notation

Different contexts signal "this is hex" differently:

- `0x` — the C-style prefix used in most programming languages: `0x1A3F`.
- `#` — used in web/CSS colors: `#FF8800`.
- `h` suffix — used in some assembly dialects: `1A3Fh`.
- `$` — used in some assembly and reverse-engineering tools: `$1A3F`.

## Where Hex Appears

- **Memory addresses** — pointers and addresses are almost always printed in hex (`0x7ffe2b…`).
- **Colors** — `#RRGGBB`: two hex digits each for red, green, and blue.
- **Raw bytes** — hex dumps of files and packets show each byte as two hex digits.
- **ASCII / Unicode** — character code points are written in hex (e.g. `0x41` = `A`).
- **MAC addresses** — `AA:BB:CC:DD:EE:FF`.
- **Hashes and checksums** — digest output (SHA, MD5) is conventionally hex.
- **Shellcode and machine code** — opcodes and payloads are read as hex bytes.

## Why This Matters for Hacking

Reading memory dumps, network captures, shellcode, and binary file contents all require being able to look at a string like `48 65 6C 6C 6F` and recognize both the bytes and their meaning (that example spells "Hello" in ASCII). Hex is the lens through which raw data becomes readable.

## See Also

- [Binary](binary.md)
- [Numeric PIN ciphering](../numeric-pin-ciphering.md) — alphanumeric codes expand the symbol set; the combination math is `base^length`, and hex is simply base-16.
