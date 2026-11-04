# Numeric PIN Security: Combinatorics, Threat Models, and Encoding Schemes

## Table of Contents

1. [Introduction and Scope](#1-introduction-and-scope)
2. [Terminology](#2-terminology)
3. [Threat Models](#3-threat-models)
4. [Combinatorics of Numeric PINs](#4-combinatorics-of-numeric-pins)
5. [Why Short PINs Can Still Be Secure: Lockouts and Rate Limiting](#5-why-short-pins-can-still-be-secure-lockouts-and-rate-limiting)
6. [Per-Length Analysis](#6-per-length-analysis)
7. [Alphanumeric Passcodes (and Why They Are Not PINs)](#7-alphanumeric-passcodes-and-why-they-are-not-pins)
8. [User-Side Encoding Schemes and Their Weaknesses](#8-user-side-encoding-schemes-and-their-weaknesses)
9. [Standards and Further Reading](#9-standards-and-further-reading)

---

## 1. Introduction and Scope

A Personal Identification Number (PIN)is a short numeric code that is used to authenticate users to devices, cards, or services.

PIN numbers are a common form of security used to protect sensitive information on electronic devices. They also serve as a form of security with physical devices such as locks and safes.

PINs are ubiquitous in our daily lives: We use them for payment cards, mobile phones, door locks, safes, and building access systems everywhere around us.

This note covers:

- **(a)** A description of the combinatorics of numeric PINs, that is, how many candidates exist for a given length and alphabet, and what that will mean in bits of entropy.
- **(b)** Threat models under which PIN length is actually consequential, as well as why lockout policy, and not raw keyspace, is the primary security control for short pins.
- **(c)** Encoding and mnemonic schemes users employ to choose and recall PINs, and why most of them weaken rather than strengthen security.

This note doesn't cover cryptographic PIN block formats (ISO 9564-1), hardware secure elements, or the internals of EMV offline PIN verification beyond a brief mention.

Consider: PINs are *not* a cryptographic key or password in the NIST sense. It is a low-entropy authenticator whose security is dependant entirely on the protocols that surround it. This means attempt counters, lockouts, secure channels, and tamper-resistant hardware. A discussion of "PIN security" that ignores this is measuring the wrong thing.

---

## 2. Terminology

| Term | Meaning in this document |
|---|---|
| **PIN** | Personal Identification Number — a short *numeric* secret. |
| **Passcode / password** | A secret that may include letters or symbols. Not a PIN. |
| **Keyspace** | The set of all possible values for a secret of given length and alphabet. |
| **Entropy (bits)** | log₂(keyspace). Measures how hard a uniformly random secret can be to guess. |
| **Online attack** | Scenario of guessing against a live system that can rate-limit, lock out, or log attempts. |
| **Offline attack** | Scenario of guessing against captured data (like a hash, an extracted keypad firmware image, a cloned token) without a rate limit. |
| **Encoding scheme** | A user-side mapping (e.g., letters → keypad digits) used to derive a PIN from something memorable. Not encryption. |
| **Lockout policy** | The rule governing how many failed attempts are permitted before the authenticator is disabled. |

---

## 3. Threat Models

It's not possible to evaluate the security of a PIN without stating who the attacker is going to be and taking into consideration what access they have. We will take a look at four relevant models.

### 3.1 Shoulder surfer / observer

Consider an attacker who is able to observe and record the PIN as it is entered. The appropriate defense is obscuring keypads, randomizing the layout, using biometric and PIN combinations, as well as physical shielding. The PIN length is almost irrelevant against this kind of attacker, as the privilege granted from observation even once is as serious with a 12 digit PIN as a 4 digit one.


### 3.2 Thief in possession of the physical device (online guessing)

Consider an attacker who has taken a card, phone, or physical lock, and is actively guessing against the live device. Defenses here are critical: Attempt counters, lockouts, delay escalation, card retention, and in the case of cards: offline PIN verification inside a secure element that sets a hard tamper-resistant limit, often 3.

In this model, short PINs are actually safe, since the protocol itself, not the key space, places a boundary on the number of guesses. A 4-digit PIN with a 3-attempt lockout gives an attacker a 3/10,000 ≈ 0.03% chance per card.

### 3.3 Offline attacker with captured data

Consider an attacker who has obtained a hash, firmware image, or a database dump and is able to test candidates at a rapid speed without a rate limit to hold them back. Defenses to rely here on are things like key stretching (PBKDF2, Argon2, scrypt), salted hashes, and hardware-bound keys. In this model, short PINs are inadequate. See §4.3 for why 4- and 6-digit PINs are trivially cracked offline.

### 3.4 Insider / database attacker

Consider an attacker with access to read the backend storage of PIN-derived data. Apply the same defenses as §3.3, and also apply the principle that PINs shouldn't ever be stored in a state that they can be reversed.

---

## 4. Combinatorics of Numeric PINs

### 4.1 Keyspace

Given a PIN length *n* drawn from an alphabet of size *k* (with repetition allowed):

**Keyspace = kⁿ**

For decimal digits, *k* = 10:

| Length | Keyspace (10ⁿ) | Entropy (bits) |
|---:|---:|---:|
| 3 | 1,000 | 9.97 |
| 4 | 10,000 | 13.29 |
| 5 | 100,000 | 16.61 |
| 6 | 1,000,000 | 19.93 |
| 8 | 100,000,000 | 26.58 |
| 10 | 10,000,000,000 | 33.22 |
| 12 | 1,000,000,000,000 | 39.86 |

To make a comparison, a 128-bit cryptographic key has 2¹²⁸ ≈ 3.4 × 10³⁸ possible values. Even a 12-digit PIN has roughly one ten-millionth of a trillionth of the entropy of a 128-bit key. This is the quantitative reason PINs are authenticators rather than just keys.

### 4.2 Restricted alphabets

A PIN drawn from a smaller alphabet *k* < 10 can still be secure, as long as it's long enough:

| Alphabet size | Length | Keyspace | Entropy (bits) |
|---:|---:|---:|---:|
| 4 (digits 0–3) | 10 | 1,048,576 | 20.00 |
| 4 | 16 | 4,294,967,296 | 32.00 |
| 4 | 20 | 1.10 × 10¹² | 40.00 |
| 8 | 10 | 1,073,741,824 | 30.00 |

Notice that a **10-digit PIN over 0–3 has roughly the same keyspace as a 6-digit decimal PIN** (1,048,576 vs 1,000,000 in the same order of magnitude). 

These kinds of restricted-alphabet schemes only help when the *length* compensates for the reduced alphabet.

### 4.3 Offline cracking time

The following makes the assumption that a single modern CPU core hashing at ~10⁶ guesses/sec (conservative for fast hashes; Argon2 with strong parameters reduces this by 3–5 orders of magnitude). A GPU cluster reaches 10⁹–10¹⁰ guesses/sec against fast hashes.

| PIN length | Keyspace | Time @ 10⁶/s | Time @ 10¹⁰/s |
|---:|---:|---:|---:|
| 4 | 10,000 | 0.01 s | 1 µs |
| 6 | 1,000,000 | 1 s | 100 µs |
| 8 | 100,000,000 | 100 s | 10 ms |
| 10 | 10¹⁰ | ~2.8 hours | 1 s |
| 12 | 10¹² | ~11.6 days | 100 s |

**In Conclusion:** Considering an offline threat model, *there is no practical PIN length that is safe* against an unkeyed fast hash. The real defense is key stretching and hardware binding, not length. A 12-digit PIN is not a substitute for a proper KDF.

---

## 5. Why Short PINs Can Still Be Secure: Lockouts and Rate Limiting

The keyspace of a 4-digit length pin is small, so it's security is dependent entirely on the attempt budget available to the attacker. With a 4-digit pin, and a 3-attempt lockout, the attacker's probability of success is 3/10,000 = 0.03% per device. With 10 attempts, 0.1%. This is the mechanism that makes bank cards safe in practice.

Typical policies:

| System | Attempts before lockout | Lockout behavior |
|---|---:|---|
| EMV payment card (offline PIN) | 3 | Card blocked; unlock at issuer |
| Mobile phone SIM | 3 | SIM blocked; PUK required (10 attempts) |
| iOS / Android device | 5–10 (configurable) | Delay escalation; optional wipe |
| Door lock / safe | 3–5 | Time delay, alarm, or lockout |

Design implications:

- **Length and lockout work as substitutes, not complements.** A 6-digit PIN with no lockout is less effective than a 4-digit PIN with a 3-attempt lockout.
- **Delay escalation** (e.g., 30 s after 3 failures, 5 min after 5) is often more usable than hard lockout and provides comparable protection against sufficiently unmotivated attackers.
- **Secure-element enforcement** matters: if the attempt counter is stored in rewritable memory, an attacker can reset it.

---

## 6. Per-Length Analysis

### 6.1 3-Digit PINs

- **Keyspace:** 1,000. **Entropy:** ~10 bits.
- **Use cases:** Legacy luggage locks, some bike locks, low-value access control.
- **Security:** Insecure as a sole authenticator against a motivated online attacker unless the system enforces a very tight lockout (e.g., 3 attempts). Even given this, 3/1000 = 0.3% per device is 10× worse than a 4-digit PIN. Against an offline attacker, this is trivial.
- **Recommendation:** Avoid.

### 6.2 4-Digit PINs

- **Keyspace:** 10,000. **Entropy:** ~13.3 bits.
- **Use cases:** Payment cards, phone locks, door locks.
- **Security:** Safe *only* given a strict lockout is in place (typically 3 attempts) enforced in tamper-resistant hardware. Offline, a 4-digit PIN is cracked in milliseconds against a fast hash.
- **Known attacks:** shoulder surfing; smudge/thermal attacks on touchscreens, birthday and anniversary guessing (see §8).
- **Recommendation:** Acceptable during card-present and device-unlock use *with* a lockout. Not acceptable as a standalone secret protecting stored data.

### 6.3 5-Digit PINs

- **Keyspace:** 100,000. **Entropy:** ~16.6 bits.
- **Use cases:** Rare in consumer systems; occasionally seen in enterprise access control and some national ID schemes.
- **Security:** An improvement over 4 digits (10× the keyspace) but subject to the same qualitative conclusion: safe online with lockout, unsafe offline. The 5-digit length is a poor ergonomic/security tradeoff — if you are going to lengthen, go to at least 6.
- **Recommendation:** Not recommended as a design choice; prefer 4 (with lockout) or 6.

### 6.4 6-Digit PINs

- **Keyspace:** 1,000,000. **Entropy:** ~19.9 bits.
- **Use cases:** iOS device passcodes, some banking apps, Google Account PIN, Windows Hello PIN.
- **Security:** Comfortably secure against online guessing with lockout in place (3/10⁶ = 0.0003% per device). Still unsafe assuming offline against a fast hash, this means 10⁶ candidates crack in about a second on a single CPU core. Mobile OSes are able to mitigate this by deriving the unlock key from the PIN as well as a hardware-bound secret stored in the secure enclave, alongside deliberate slowdowns, so that offline extraction of the enclave key is required.
- **Known attacks:** shoulder surfing; pattern-of-use guessing (dates, repeated digits); offline extraction of the secure-enclave binding (a high-effort, targeted attack).
- **Recommendation:** The best choice to default to for device unlock. Pair with a secure element and delay escalation.

### 6.5 Longer PINs (8+ digits) *(TODO)*

Expand with: use in some national ID and banking schemes; the point of diminishing returns given lockout; the fact that length cannot substitute for a KDF in offline scenarios.

---

## 7. Alphanumeric Passcodes (and Why They Are Not PINs)

Definitially, a string which contains letters and digits is a passcode or password, not a PIN as such. This distinction is significant because the properties that make it secure and the application standards differ.

Consider:

- **Case-sensitive alphanumeric, length 4:** 62⁴ ≈ 14.8 million candidates (~23.8 bits).
- **Uppercase-only alphanumeric, length 4:** 36⁴ ≈ 1.68 million candidates (~20.7 bits).
- **Case-sensitive alphanumeric, length 8:** 62⁸ ≈ 2.18 × 10¹⁴ (~47.6 bits).

Here we see a meaningful difference in the sizing of the keyspace in comparison to that of a numeric pin of the same length. Yet, as §4.3 shows, 24 bits is still trivially breakable offline. The lesson is the same: **the KDF and the lockout policy are what matter, not the alphabet.**

---

## 8. User-Side Encoding Schemes and Their Weaknesses

Users are prone to derive PINs from something memorable to them. This can be misinterpreted as ciphering, but is not actually crytographic by nature. They can even reduce the security by reducing the keyspace to something guessable by a human.

### 8.1 Phone-keypad letter mapping

Mapping letters to digits on a standard telephone keypad (ABC→2, DEF→3, …, WXYZ→9) allows a word to be entered as a digit string: e.g., `HELLO` → `43556`, `PASSWORD` → `72779673`.

This has no meaningful application to securing the data we wish to protect.

### 8.2 Date-based PINs

Birthdays, anniversaries, and graduation years (e.g., `0714`, `1985`) are among the most common 4-digit PINs. Studies of leaked PIN sets (e.g., the 2012 DataGenetics analysis of 3.4 million disclosed 4-digit PINs) show that the top 20 PINs account for over 26% of all real-world PINs, with `1234`, `1111`, `0000`, and date-like patterns dominating.

We can offer some mitigation by using systems to reject known-weak PINs at enrollment (blocklists of the top 10,000, repeated-digit and sequential patterns, and date-like ranges).

### 8.3 Sequential and repeated-digit patterns

`1234`, `4321`, `0000`, `1111`, `1212`, `2580` (vertical keypad column) and similar patterns are heavily over-represented. Enrollment-time blocklists should reject them.

### 8.4 "Ciphered" PINs (e.g., ROT-style shifts)

Sometimes, users apply mental transformations to a basic PIN, (add 1 to each digit, reverse the string, etc.).

This will provide no security benefit against an attacker who is aware of this kind of scheme, and negligable benefit against one who isn't.

### 8.5 Summary

| Scheme | Effective entropy | Verdict |
|---|---|---|
| Random 4-digit | ~13.3 bits (ideal) | Acceptable with lockout |
| Keypad mnemonic (common word) | ~10–15 bits, dictionary-attackable | Weak |
| Date-based | ~6–10 bits (constrained range) | Weak; blocklist |
| Sequential / repeated | ~3–5 bits | Reject at enrollment |
| "Ciphered" transformation | ≈ base scheme | No benefit |

---

## 9. Standards and Further Reading

- **ISO 9564-1** — Financial services: Personal Identification Number (PIN) management and security. Defines PIN block formats (ISO 9564-1 format 0, format 3, etc.), PIN encryption, and offline PIN verification requirements.
- **PCI PIN Security Requirements** — PCI SSC requirements for PIN-entry devices, key management, and PIN translation in payment environments.
- **EMV Specifications** — Book 1 (ICC to terminal interface) and Book 4 (security) cover offline PIN verification and the 3-attempt lockout behavior.
- **NIST SP 800-63B** — Digital Identity Guidelines: Authentication and Lifecycle Management. §5.1.1 covers memorized secrets; while aimed at passwords, the guidance on rate limiting, blocklists, and entropy is directly applicable.
- **NIST SP 800-132** — Recommendation for Password-Based Key Derivation (PBKDF2), relevant to §4.3's offline-attack discussion.
- **DataGenetics, "PIN analysis" (2012)** — Analysis of 3.4 million disclosed 4-digit PINs; the canonical reference for real-world PIN distribution.
- **RFC 9106** — Argon2 memory-hard password hashing (for the KDF side of PIN verification).