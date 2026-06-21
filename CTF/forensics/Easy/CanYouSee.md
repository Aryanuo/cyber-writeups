# CanYouSee

**Platform:** picoCTF / CyLab 2024
**Category:** Forensics
**Difficulty:** Easy
**Author:** Mubarak Mikail
**Challenge Link:** [CanYouSee](https://learn.cylabacademy.org/library/408?page=1&search=canyou)

---

## Challenge Description

> How about some hide and seek?

Download the provided file and recover the hidden flag.

### Hints

1. How can you view the information about the picture?
2. If something isn't in the expected form, maybe it deserves attention?

---

# Objective

* Examine the image metadata.
* Identify any suspicious or hidden information.
* Recover the hidden flag.

---

# Initial Analysis

The challenge provides a ZIP archive containing an image.

Extract the archive:

```bash
unzip unknown.zip
```

Output:

```text
Archive:  unknown.zip
  inflating: ukn_reality.jpg
```

The extracted file is:

```text
ukn_reality.jpg
```

---

# Solution

## Step 1 - Inspect Image Metadata

The first hint suggests examining the image information.

`exiftool` is commonly used to view metadata stored inside image files.

```bash
exiftool ukn_reality.jpg
```

Output (trimmed):

```text
Attribution URL : cGljb0NURntNRTc0RDQ3QV9ISUREM05fNmE5ZjVhYzR9Cg==
```

One metadata field immediately stands out.

The value consists only of:

* Uppercase letters
* Lowercase letters
* Numbers
* `=`

This strongly suggests that the data is **Base64 encoded**.

---

## Step 2 - Decode the Metadata

Decode the Base64 string using:

```bash
echo 'cGljb0NURntNRTc0RDQ3QV9ISUREM05fNmE5ZjVhYzR9Cg==' | base64 -d
```

Output:

```text
picoCTF{REDACTED}
```

The decoded text reveals the flag.

---

# Flag

```text
picoCTF{TRY IT}
```

---

# Why the Attack Works

Image files often contain metadata such as:

* Camera information
* Creation dates
* GPS coordinates
* Copyright details
* Custom metadata fields

Challenge creators frequently hide information inside these metadata fields because they are not visible when viewing the image normally.

In this challenge, the flag was stored inside the **Attribution URL** metadata field as a Base64-encoded string.

By inspecting the metadata with `exiftool` and decoding the encoded value, the hidden flag could be recovered.

---

# Key Takeaways

* Always inspect metadata during forensic challenges.
* `exiftool` is one of the most useful tools for metadata analysis.
* Base64 is an encoding format, not encryption.
* Metadata fields can contain hidden information beyond standard image properties.
* Pay close attention to challenge hints—they often point directly to the intended solution.

---

# Tools Used

* unzip
* exiftool
* base64
* Linux Terminal
