# hashcrack

**Platform:** picoCTF / CyLab 2025
**Category:** Cryptography
**Difficulty:** Easy
**Author:** Nana Ama Atombo-Sackey
**Challenge Link:** https://learn.cylabacademy.org/library/475?page=1&category=2

---

## Challenge Description

> A company stored a secret message on a server which got breached due to the admin using weakly hashed passwords. Can you gain access to the secret stored within the server?

> Additional details appear once you launch your instance.

---

## Hints

1. Understanding hashes is very crucial.
2. Can you identify the hash algorithm? Look carefully at the length and structure of each hash identified.
3. Tried using any hash cracking tools?

---

# Objective

Identify the hash algorithms being used, recover the original plaintext values, and use them to obtain the flag.

---

# Background

## What is Hashing?

Hashing is a one-way process that converts data into a fixed-length string known as a hash or digest.

Unlike encryption, hashing is designed to be irreversible. However, weak passwords and commonly used words can often be recovered using:

* Rainbow tables
* Dictionary attacks
* Online hash databases

This challenge demonstrates why using weak passwords with unsalted hashes is insecure.

---

# Initial Analysis

Connect to the challenge instance:

```bash
nc verbal-sleep.picoctf.net 60001
```

After connecting, the server presents three hashed values and requests the corresponding plaintext values.

The challenge does not require breaking the hashing algorithms themselves. Instead, the hashes correspond to common words that can be recovered using publicly available hash databases.

---

# Solution

## Step 1: Connect to the Server

```bash
AryanCtF-academy@webshell:~$ nc verbal-sleep.picoctf.net 60001
Welcome!! Looking For the Secret?

We have identified a hash: 482c811da5d5b4bc6d497ffa98491e38
Enter the password for identified hash: 
```

The server displays a hash and waits for input.

---

## Step 2: Identify the Hash Type

Determine the hashing algorithm based on the length of the hash.

### MD5 Example

```text
32 hexadecimal characters
```

### SHA-1 Example

```text
40 hexadecimal characters
```

### SHA-256 Example

```text
64 hexadecimal characters
```

---

## Step 3: Recover the Plaintext

Copy each hash and search it using an online hash database such as [Crackstation](https://crackstation.net/).

Paste the hash into the search field and retrieve the original plaintext value.

Repeat this process for all three hashes.
```bash
AryanCtF-academy@webshell:~$ nc verbal-sleep.picoctf.net 60001
Welcome!! Looking For the Secret?

We have identified a hash: 482c811da5d5b4bc6d497ffa98491e38
Enter the password for identified hash: password123
Correct! You've cracked the MD5 hash with no secret found!

The MD5-hash is known by cracstation and the corresponing password is 'password123'.

Flag is yet to be revealed!! Crack this hash: b7a875fc1ea228b9061041b7cec4bd3c52ab3ce3
Enter the password for the identified hash: letmein
Correct! You've cracked the SHA-1 hash with no secret found!

4This sha1-hash is also known by Crackstation and the corresponding password is `letmein`.

Almost there!! Crack this hash: 916e8c4f79b25028c9e467f1eb8eee6d6bbdff965f9928310ad30a8d88697745
Enter the password for the identified hash: qwerty098
Correct! You've cracked the SHA-256 hash with a secret found. 

This sha256-hash is also known by Crackstation and the corresponding password is `qwerty098`.
The flag is: picoCTF{Redacted}
```
---

## Step 4: Submit the Answers

Enter each recovered plaintext value when prompted by the server.

After successfully solving all three hashes, the server reveals the flag.

---

# Flag

```text
picoCTF{REDACTED}
```

---

# Why the Attack Works

Hashing algorithms such as MD5, SHA-1, and SHA-256 are not inherently broken in this challenge.

The weakness lies in the use of predictable and commonly used passwords.

When users choose weak passwords:

* Their hashes may already exist in public databases.
* Attackers can use rainbow tables and dictionary attacks.
* Recovering the original plaintext becomes trivial.

This is why modern systems use:

* Strong passwords
* Unique salts
* Password hashing algorithms such as bcrypt, scrypt, or Argon2

---

# Key Takeaways

* Hash length can often reveal the hashing algorithm being used.
* Weak passwords can be recovered even when strong hashing algorithms are used.
* Online hash databases are effective against common passwords.
* Unsalted hashes are vulnerable to rainbow table attacks.
* Proper password storage requires both strong hashing algorithms and salting.

---

# Tools Used

* Netcat (`nc`)
* CrackStation
* Linux Terminal
