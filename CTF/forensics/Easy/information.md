# Information

**Platform:** picoCTF / CyLab 2021
**Category:** Forensics
**Difficulty:** Easy
**Author:** susie
**Challenge Link:** [Information](https://learn.cylabacademy.org/library/186?page=1&search=inf)

---

## Challenge Description

> Files can always be changed in a secret way. Can you find the flag?

### Files Provided

* `cat.jpg`

### Hints

1. Look at the details of the file.
2. Make sure to submit the flag as `picoCTF{XXXXX}`.

---

# Objective

* Examine the metadata of the provided image.
* Identify any suspicious information.
* Recover the hidden flag.

---

# Initial Analysis

The challenge provides a single image named:

```text
cat.jpg
```

The hint suggests examining the file's details, making metadata analysis the logical first step.

---

# Solution

## Step 1 - Examine the Metadata

Use `exiftool` to inspect the image metadata:

```bash
exiftool cat.jpg
```

Relevant output:

```text
License : cGljb0NURnt0aGVfbTN0YWRhdGFfMXNfbW9kaWZpZWR9
```

The **License** field immediately stands out because it contains a long string composed of letters and numbers, ending without any readable text.

This strongly suggests that the value is **Base64 encoded**.

---

## Step 2 - Decode the Encoded String

Decode the Base64 value:

```bash
echo 'cGljb0NURnt0aGVfbTN0YWRhdGFfMXNfbW9kaWZpZWR9' | base64 -d
```

Output:

```text
picoCTF{REDACTED}
```

The decoded text reveals the flag.

---

# Flag

```text
picoCTF{REDACTED}
```

---

# Why the Attack Works

Image files often contain metadata such as:

* Camera information
* Copyright details
* GPS coordinates
* Author information
* Custom metadata fields

These metadata fields are not visible when viewing the image normally, making them a common place to hide information during forensic challenges.

In this challenge, the flag was hidden inside the **License** metadata field as a Base64-encoded string. By inspecting the metadata with `exiftool` and decoding the value, the hidden flag could be recovered.

---

# Key Takeaways

* Metadata is one of the first places to inspect during forensic investigations.
* `exiftool` is a powerful utility for viewing and analyzing file metadata.
* Base64 is an encoding format, not encryption, and can be decoded easily.
* Always investigate unusual or unreadable metadata fields—they may contain hidden information.
* Challenge hints often indicate the intended investigation path.

---

# Tools Used

* exiftool
* base64
* Linux Terminal
