# extensions

**Platform:** picoCTF / CyLab 2019
**Category:** Forensics
**Difficulty:** Medium
**Author:** Sanjay C / Danny
**Challenge Link:** [extensions](https://play.picoctf.org/practice/challenge/55)

---

## Challenge Description

> This is a really weird text file. Can you find the flag?

### Files Provided

* `flag.txt`

### Hints

1. How do operating systems know what kind of file it is? (It's not just the ending!)
2. Make sure to submit the flag as `picoCTF{XXXXX}`.

---

# Objective

* Identify the actual file type.
* Recover the hidden flag.

---

# Solution

## Step 1 - Inspect the File

Viewing the file with `cat` displays unreadable data, indicating that it is not a normal text file.

To determine its actual file type, use the `file` command:

```bash
file flag.txt
```

Output:

```text
flag.txt: PNG image data, 1697 x 608, 8-bit/color RGB, non-interlaced
```

Although the file has a `.txt` extension, it is actually a **PNG image**.

---

## Step 2 - Rename the File

Rename the file with the correct extension:

```bash
mv flag.txt flag.png
```

---

## Step 3 - Open the Image

Open the renamed image using any image viewer.

The image displays the flag.

---

# Flag

```text
picoCTF{REDACTED}
```

---

# Why the Attack Works

Operating systems do not rely solely on a file's extension to determine its type.

Most file formats contain a **file signature** (also called a **magic number**) at the beginning of the file. The `file` utility examines these signatures to identify the true file type.

In this challenge, the file was disguised with a `.txt` extension even though its contents were a valid PNG image.

---

# Key Takeaways

* File extensions can be misleading.
* The `file` command identifies a file based on its internal signature rather than its name.
* Always verify a file's actual type during forensic investigations.
* Renaming a file with the correct extension can reveal hidden content.

---

# Tools Used

* file
* mv
* Linux Terminal
