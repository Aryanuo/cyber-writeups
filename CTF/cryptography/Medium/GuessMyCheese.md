# Guess My Cheese (Part 1)

**Platform:** picoCTF / CyLab 2025  
**Category:** Cryptography  
**Difficulty:** Medium  
**Author:** Aditin  
**Challenge Link:** https://learn.cylabacademy.org/library/473?page=1&search=guess

---

## Challenge Description

> Try to decrypt the secret cheese password to prove you're not the imposter!

### Hint

> Remember that cipher we devised together Squeexy? The one that incorporates your affinity for linear equations???

The hint suggests that the encryption method may involve a mathematical equation, making the Affine Cipher a likely candidate.

---

# Objective

Determine the encryption scheme used by the service, recover the encrypted cheese name.

---

# Background

## Affine Cipher

The Affine Cipher is a substitution cipher that uses a linear equation to transform plaintext characters.

Encryption:

```text id="tjlwmr"
C = (aP + b) mod 26
```

Decryption:

```text id="mrjq7x"
P = a⁻¹(C - b) mod 26
```

Where:

* `P` = Plaintext character value
* `C` = Ciphertext character value
* `a` = Multiplicative key
* `b` = Additive key
* `a⁻¹` = Modular inverse of `a`

Unlike a Caesar cipher, the Affine Cipher uses two keys, making frequency analysis slightly more difficult.

---

# Initial Analysis

After launching the challenge instance, a Netcat service is provided:

```bash id="lmwgsn"
nc verbal-sleep.picoctf.net 56615
```

Upon connecting, the service presents an encrypted cheese:

```text id="v2f1cm"
Here's my secret cheese -- if you're Squeexy, you'll be able to guess it:

WYLSMZZMLXM
```

The service provides two options:

```text id="hl4ksg"
(g)uess my cheese
(e)ncrypt a cheese
```

Since the encryption method is unknown, the encryption functionality can be used as an oracle to gather plaintext-ciphertext pairs.

---

# Solution

## Step 1 - Test the Encryption Function

Attempting to encrypt a random string fails:

```text id="d0kdmr"
What cheese would you like to encrypt? ABCD

I'm sorry I haven't had that cheese before,
so I can't encrypt it!
```

This indicates that the service only accepts predefined cheese names.

---

## Step 2 - Obtain a Plaintext-Ciphertext Pair

Submit a common cheese name:

```text id="lwj4kh"
MOZZARELLA
```

The service responds with:

```text id="guflhc"
Here's your encrypted cheese:

OIBBYZMRRY
```

This gives us a known pair:

```text id="ksglnx"
Plaintext  : MOZZARELLA
Ciphertext : OIBBYZMRRY
```

---

## Step 3 - Identify the Cipher

The challenge hint references linear equations, which strongly suggests an Affine Cipher.

Using the known plaintext-ciphertext pair, navigate to:

https://www.dcode.fr/affine-cipher

Select:

```text id="m6d2or"
Automatic Brute Force Decryption
```

and enter:

```text id="3ypsvl"
Ciphertext: OIBBYZMRRY
```

dCode successfully identifies the Affine Cipher parameters and reveals the values of:

```text id="2e2ujd"
a
b
```

### Verification

To verify the result, use the Affine Cipher encoder on dCode.

Entering:

```text id="wud4r8"
MOZZARELLA
```

produces:

```text id="txwgbm"
OIBBYZMRRY
```

confirming that the identified key values are correct.

---

## Step 4 - Decrypt the Secret Cheese

Choose the guess option:

```text id="v0bfr0"
g
```

The service again displays the encrypted cheese:

```text id="3v2yfq"
WYLSMZZMLXM
```

Using the previously identified Affine Cipher parameters, decrypt the ciphertext using dCode.

The resulting plaintext is:

```text id="eojv7n"
SANCERRENJE
```

---

## Step 5 - Submit the Cheese

Provide the decrypted cheese name when prompted:

```text id="n9vqhj"
So...what's my cheese?

SANCERRENJE
```

The service accepts the answer and reveals the password to the cloning room.

---

## Flag

```text id="2q8u5o"
picoCTF{REDACTED}
```

---

# Why the Attack Works

The challenge exposes an encryption oracle through the `(e)ncrypt` option.

By supplying a valid cheese name, it is possible to obtain a plaintext-ciphertext pair:

```text id="m8pgko"
MOZZARELLA → OIBBYZMRRY
```

This pair provides enough information to identify the Affine Cipher parameters.

Once the key values are known, any ciphertext produced by the service can be decrypted, including the secret cheese required to complete the challenge.

The vulnerability is not in the Affine Cipher itself but in providing an encryption oracle that leaks information about the underlying encryption scheme.

---

# Key Takeaways

* Known plaintext-ciphertext pairs are extremely valuable in cryptanalysis.
* Challenge hints often provide important clues about the underlying cipher.
* Encryption oracles can leak enough information to identify and break classical ciphers.
* Affine Cipher encryption uses two keys instead of one, making it slightly stronger than Caesar Cipher.
* Tools such as dCode are useful for quickly identifying and testing classical cryptographic algorithms.

---

# Tools Used

* Netcat (`nc`)
* dCode Affine Cipher Tool
* Linux Terminal

---

# References

* [dCode Affine Cipher Tool](https://www.dcode.fr/affine-cipher)
* [Affine Cipher](https://en.wikipedia.org/wiki/Affine_cipher)
