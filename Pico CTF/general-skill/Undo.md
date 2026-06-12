# Undo

**Platform:** picoCTF / CyLab 2026
**Category:** General Skills
**Difficulty:** Easy
**Author:** Yahaya Meddy
**Challenge Link:** https://learn.cylabacademy.org/library/766?page=1

---

## Challenge Description

> Can you reverse a series of Linux text transformations to recover the original flag?

### Hint

For text translation and character replacement, see the `tr` command documentation:

https://man7.org/linux/man-pages/man1/tr.1.html

---

# Objective

Identify the text transformation applied at each stage and provide the Linux command required to reverse it.

---

# Initial Analysis

After launching the instance, a Netcat service is provided:

```bash
nc foggy-cliff.picoctf.net 57532
```

The challenge presents a transformed version of the flag along with a hint describing the operation that was performed.

To progress, the correct Linux command must be entered to reverse the most recent transformation.

---

# Solution

## Step 1 - Base64 Decoding

Connect to the challenge:

```bash
┌──(kali㉿kali)-[~]
└─$ nc foggy-cliff.picoctf.net 54167
===Welcome to the Text Transformations Challenge!===

Your goal: step by step, recover the original flag.
At each step, you'll see the transformed flag and a hint.
Enter the correct Linux command to reverse the last transformation.

--- Step 1 ---
Current flag: KXM5MzA0MG5zLWZhMDFnQHplMHNmYTRlRy1nazNnLXRhMWZlcmlyRShTR1BicHZj
Hint: Base64 encoded the string.
Enter the Linux command to reverse it:
```

The hint indicates that the text was Base64 encoded.

To reverse Base64 encoding:

```bash
base64 -d
```

```text
Correct!
```

---

## Step 2 - Reverse the Text

```text
--- Step 2 ---
Current flag: )s93040ns-fa01g@ze0sfa4eG-gk3g-ta1ferirE(SGPbpvc
Hint: Reversed the text.
Enter the Linux command to reverse it:
```

The string has been reversed.

To restore the original order:

```bash
rev
```

```text
Correct!
```

---

## Step 3 - Replace Dashes with Underscores

```text
--- Step 3 ---
Current flag: cvpbPGS(Eriref1at-g3kg-Ge4afs0ez@g10af-sn04039s)
Hint: Replaced underscores with dashes.
Enter the Linux command to reverse it:
```

The hint states that underscores were replaced with dashes.

To reverse the transformation:

```bash
tr '-' '_'
```

```text
Correct!
```

---

## Step 4 - Replace Parentheses with Curly Braces

```text
--- Step 4 ---
Current flag: cvpbPGS(Eriref1at_g3kg_Ge4afs0ez@g10af_sn04039s)
Hint: Replaced curly braces with parentheses.
Enter the Linux command to reverse it:
```

The original curly braces were replaced with parentheses.

To restore them:

```bash
tr '()' '{}'
```

```text
Correct!
```

---

## Step 5 - Reverse ROT13

```text
--- Step 5 ---
Current flag: cvpbPGS{Eriref1at_g3kg_Ge4afs0ez@g10af_sn04039s}
Hint: Applied ROT13 to letters.
Enter the Linux command to reverse it:
```

ROT13 is a substitution cipher that shifts each letter by 13 positions.

Applying ROT13 again reverses the transformation.

```bash
tr 'A-Za-z' 'N-ZA-Mn-za-m'
```

```text
Correct!
```

---

## Flag Retrieval

After successfully reversing all transformations:

```text
Congratulations! You've recovered the original flag:

>>> picoCTF{REDACTED}
```

---

# Why the Challenge Works

This challenge applies a series of reversible text transformations to the original flag.

The transformations are:

1. Base64 Encoding
2. Text Reversal
3. Character Replacement (`_` → `-`)
4. Character Replacement (`{}` → `()`)
5. ROT13 Encoding

Each transformation has a corresponding Linux command that can undo it.

By recognizing the operation described in each hint and applying the appropriate reverse transformation, the original flag can be recovered.

---

# Key Takeaways

* Base64 is an encoding scheme, not encryption, and can be reversed using `base64 -d`.
* The `rev` command reverses text character-by-character.
* The `tr` command can translate and replace characters within a string.
* ROT13 is a symmetric substitution cipher; applying ROT13 twice restores the original text.
* Understanding Linux text-processing utilities is valuable for CTF challenges and automation tasks.
* Many beginner CTF challenges focus on recognizing common transformations rather than exploiting vulnerabilities.

---

# Tools Used

* Netcat (`nc`)
* `base64`
* `rev`
* `tr`
* Linux Terminal
