# Easy1

**Platform:** picoCTF 2019
**Category:** Cryptography
**Difficulty:** Medium
**Author:** Alex Fulton / Danny

## Challenge Description

> The one time pad can be cryptographically secure, but not when you know the key.
>
> Can you solve this?
>
> We've given you the encrypted flag, key, and a table to help decrypt `UFJKXQZQUNB` with the key `SOLVECRYPTO`.
>
> Can you use this table to solve it?

### Hints

1. Submit your answer in the format `picoCTF{FLAG}`.
2. Use uppercase letters for the message.

---

## Background: One-Time Pad (OTP)

A One-Time Pad (OTP) is an encryption technique where a plaintext message is combined with a secret key.

For OTP to be perfectly secure, the key must be:

* Truly random
* At least as long as the message
* Used only once
* Kept completely secret

If all these conditions are met, OTP is theoretically unbreakable. However, in this challenge the key is already provided, making decryption straightforward.

---

## Given Information

**Ciphertext**

```text
UFJKXQZQUNB
```

**Key**

```text
SOLVECRYPTO
```

---

## Solution

There are multiple ways to solve this challenge.

### Method 1: Online Decoder

Use an online Vigenère/OTP decoder and provide:

* Ciphertext: `UFJKXQZQUNB`
* Key: `SOLVECRYPTO`

Useful tools:

* https://www.braingle.com/brainteasers/codes/onetimepad.php
* https://rumkin.com/tools/cipher/one-time-pad/

---

### Method 2: Using the Provided Table

Take the first ciphertext and key characters:

```text
Ciphertext = U
Key        = S
```

Locate row **S** in the table and find the ciphertext letter **U**.

```text
S | S T U V W X Y Z A B C D E F G H I J K L M N O P Q R
        ^
        U
```

The letter **U** appears under column **C**.

Therefore:

```text
Ciphertext U
Key        S
Plaintext  C
```

Repeating this process for each character:


---

### Method 3: Alphabet Position Arithmetic

Convert letters to numerical positions:

```text
A = 0, B = 1, C = 2, ..., Z = 25
```

For the first characters:

```text
Ciphertext U = 20
Key S        = 18
```

Apply Vigenère decryption:

```text
P = (C - K) mod 26
```

```text
P = (20 - 18) mod 26
P = 2
```

Position `2` corresponds to:

```text
C
```

Thus:

```text
U - S = C
```

Repeating the calculation for every character:

```text

```

---

## Flag

```text
picoCTF{REDACTED}
```

---

## Key Takeaways

* A One-Time Pad is only secure when the key remains secret and is used exactly once.
* If the key is known, decryption becomes trivial.
* The challenge demonstrates the relationship between OTP-style encryption and Vigenère cipher operations.
* Understanding modular arithmetic is useful for solving classical cryptography challenges.
