# Hacking Fundamentals Overview

## Purpose

The fundamentals track is the foundation for everything else in the [hacking](../hacking-overview.md) section. Hacking is ultimately about understanding systems *below* the abstractions that normal usage presents — and the lowest abstraction of every computing system is the number. Before memory, networks, binaries, or cryptography make sense, the raw representation of data has to.

## Why Number Systems Come First

Every piece of data a computer stores, sends, or executes is ultimately a sequence of two states. Three number systems matter to a hacker:

- **Decimal (base-10)** — the system humans count in; the reference point.
- **Binary (base-2)** — how the machine actually stores and manipulates data.
- **Hexadecimal (base-16)** — the compact, human-readable shorthand for binary.

The single most important relationship on this page: **one hexadecimal digit represents exactly four binary digits (bits).** Everything else follows from that.

## Written

- [Binary](binary.md) — base-2, bits and bytes, and converting to and from decimal.
- [Hexadecimal](hexadecimal.md) — base-16, and converting to and from binary and decimal.

## Planned (stubs)

- Character encoding (ASCII, UTF-8, Unicode)
- Bitwise operations and bitmasks
- Endianness (little-endian vs big-endian)
- Boolean logic and truth tables

## Related

- [Numeric PIN ciphering](../numeric-pin-ciphering.md) — a direct application of base arithmetic: the number of combinations of a code of length *n* over *b* symbols is *bⁿ*.
- [Mathematics overview](../../mathematics/mathematics-overview.md) — number systems are a mathematical concept that programming inherits.
