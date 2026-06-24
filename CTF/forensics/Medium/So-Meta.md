# So Meta

**Platform:** picoCTF / CyLab 2019
**Category:** Forensics
**Difficulty:** Medium
**Author:** Kevin Cooper / Danny
**Challenge Link:** [So Meta](https://learn.cylabacademy.org/library/19?page=1&search=o+met)

---

## Challenge Description

> Find the flag in this picture.

### Hints

1. What does *meta* mean in the context of files?
2. Ever heard of metadata?

---

# Objective

* Inspect the image metadata.
* Recover the hidden flag.

---

# Solution

The challenge hints suggest looking at the image metadata.

Use `exiftool` to inspect the PNG file:

```bash
exiftool pico_img.png
```

The output reveals that the flag is stored directly in the **Artist** metadata field.

![ExifTool Output](screenshots/SoMeta.png)

---

# Flag

```text
picoCTF{REDACTED}
```

---

# Why the Attack Works

Image files can contain metadata such as the author, copyright information, creation date, and other custom fields.

In this challenge, the flag was hidden in the **Artist** metadata field, making it recoverable by inspecting the file with `exiftool`.

---

# Key Takeaways

* Always inspect metadata during forensic challenges.
* `exiftool` is one of the most useful tools for analyzing file metadata.
* Hidden information is often stored in standard metadata fields.

---

# Tools Used

* exiftool
* Linux Terminal
