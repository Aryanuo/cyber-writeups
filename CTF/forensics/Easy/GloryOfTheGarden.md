# Glory of the Garden

**Platform:** picoCTF / CyLab 2019
**Category:** Forensics
**Difficulty:** Easy
**Author:** jedavis / Danny
**Challenge Link:** [Glory of the Garden](https://learn.cylabacademy.org/library/44?page=1&search=glory)

---

## Challenge Description

> This file contains more than it seems.

### Files Provided

* `garden.jpg`

### Hint

1. What is a hex editor?

---

# Objective

* Inspect the provided image for hidden information.
* Recover the flag.

---

# Solution

The first step was to inspect the image metadata using `exiftool`.

```bash
exiftool garden.jpg
```

No useful metadata or hidden information was found.

Since JPEG files may contain embedded printable strings, the next step was to inspect the file using the `strings` utility.

```bash
strings garden.jpg
```

Among the output, the flag was visible in plain text.

---

# Flag

```text
picoCTF{REDACTED}
```

---

# Why the Attack Works

JPEG files often contain readable ASCII strings in addition to binary image data.

The `strings` utility extracts printable character sequences from a file, making it useful for quickly identifying embedded text that may not appear in the image itself.

In this challenge, the flag was stored as plain text inside the JPEG file and could be recovered directly using `strings`.

---

# Key Takeaways

* Metadata analysis does not always reveal hidden information.
* `strings` is a quick and effective tool for inspecting readable text within files.
* Always try multiple forensic techniques when analyzing files.

---

# Tools Used

* exiftool
* strings
* Linux Terminal
