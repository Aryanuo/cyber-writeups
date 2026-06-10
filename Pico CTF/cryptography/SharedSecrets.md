# Shared Secrets

**Platform:** picoCTF / CyLab 2026  
**Category:** Cryptography  
**Difficulty:** Easy  
**Author:** Yahaya Meddy  
[**Challenge Link:**](https://learn.cylabacademy.org/library/715?page=1&category=2&event=79)  

---

## Challenge Description

> A message was encrypted using a shared secret... but it looks like one side of the exchange leaked something. Can you piece together the secret and get the flag?

### Hint

> What do you get if you combine a public key with a known private one?

---

# Background

## Diffie–Hellman Key Exchange (DH)

Diffie–Hellman is a cryptographic protocol that allows two parties to establish a shared secret over an insecure channel without ever transmitting the secret itself.

The idea is that both parties agree on two public values:

```text
p = Prime Number
g = Generator
```

Each participant then chooses a private key.

### Alice

Alice chooses:

```text
a = Private Key
```

and computes:

```text
A = gᵃ mod p
```

She sends `A` publicly.

### Bob

Bob chooses:

```text
b = Private Key
```

and computes:

```text
B = gᵇ mod p
```

He sends `B` publicly.

### Shared Secret

Alice computes:

```text
Shared = Bᵃ mod p
```

Bob computes:

```text
Shared = Aᵇ mod p
```

Due to the properties of modular arithmetic:

```text
(Bᵃ mod p) = (Aᵇ mod p) = gᵃᵇ mod p
```

Both parties arrive at the same shared secret.

The security of Diffie–Hellman depends on keeping the private keys secret.

---

# Files Provided

* `encryption.py` – Contains the Diffie–Hellman implementation and encryption logic.
* `message.txt` – Contains the public parameters and encrypted message.

---

# Initial Analysis

Inspecting `message.txt` reveals:

```text
g = ..., p = ..., A = ..., b = ..., enc = ...
```

The critical observation is that **Bob's private key (`b`) has been leaked**.
In a secure Diffie–Hellman exchange, an attacker should only know:

```text
g, p, A, B
```

However, knowing one participant's private key allows the shared secret to be calculated directly.

Using:

```text
A = Alice's Public Key
b = Bob's Private Key
```

the shared secret can be recovered as:

```python
shared = pow(A, b, p)
```

Once the shared secret is known, the encryption key can be derived and the ciphertext decrypted.

---

# Solution

Create a Python script:

```bash
mousepad solve.py
```

Paste the following code:

```python
g = 2
p = 2549189574813286838731164889759660985718829773591105476199519705873412196312430317020838926243603568621442315899465054113173947320336232433955810978828549997135650568305743237094254970976323874275496906890604182065479938670042325573071240425116728265626285703063369264515156139785599306841009782845534345233632656299

A = 985445375040965660286925493195705022105311734388727844225279873957046781773616909873718436322630406874986079724535489250394868561673679481570937115431819833192904575232350968046861369069756001815958202523940582773045326660550531101765085548729437758998731821183288714745944939562470177082773230192655925648636693128

b = 2531748005435027320362428017101462589109367420602712788105635351744163633032425558043495896092073893867970357855640982255766093320905998447373338799811757556564212066483256443137066327562660885114225713445384050398429354624442622758857644344928037376358520893635120478670063221879818261074326351747274950092218676710

enc = bytes.fromhex(
    "4d545e527e697b465955624e0e5e4f0e49620404050f5b5b580b40"
)

shared = pow(A, b, p)

key = shared % 256

flag = bytes([x ^ key for x in enc])

print(flag.decode())
```

Save the file and execute it:

```bash
python3 solve.py
```

The script computes the shared secret using the leaked private key, derives the XOR key, and decrypts the ciphertext.

---

# Flag

```text
picoCTF{REDACTED}
```

---

# Why the Attack Works

The challenge does not exploit a weakness in the Diffie–Hellman algorithm itself.

Instead, it exploits poor key management.

The protocol assumes that private keys remain secret. Once Bob's private key is exposed, an attacker can calculate:

```python
shared = pow(A, b, p)
```

and recover the same shared secret that Alice and Bob use for encryption.

This completely breaks the confidentiality of the communication.

---

# Key Takeaways

* Diffie–Hellman is secure only when private keys remain secret.
* Possession of one private key and the other party's public key is sufficient to recover the shared secret.
* Many cryptographic challenges are solved through implementation mistakes rather than flaws in the underlying algorithm.
* Python's built-in `pow(base, exponent, modulus)` function is extremely useful for modular arithmetic and cryptography challenges.
* Always inspect provided files carefully—sensitive information is often hidden in plain sight.

---

# Tools Used

* Python 3
* Linux Command Line
