# Secret of the Polyglot

**Platform:** picoCTF / CyLab 2024
**Category:** Forensics
**Difficulty:** Easy
**Author:** syreal
**Challenge Link:** [Secret of the Polyglot](https://play.picoctf.org/practice/challenge/475)

---

## Challenge Description

> The Network Operations Center (NOC) of your local institution picked up a suspicious file. They're getting conflicting information about what type of file it is. Can you extract all the information from this strange file?

### Files Provided

* `flag2of2-final.pdf`

### Hint

1. This problem can be solved by just opening the file in different ways.

---

# Objective

* Determine the true nature of the provided file.
* Recover both parts of the flag.

---

# Solution

## Step 1 - Open the File as a PDF

Open the provided file using any PDF viewer:

```bash
xdg-open flag2of2-final.pdf
```

The PDF reveals the **second half** of the flag.

---

## Step 2 - Identify the Actual File Type

The hint suggested "opening the file in different ways". 

Use the `file` command to inspect the file:

```bash
file flag2of2-final.pdf
```

Output:

```text
flag2of2-final.pdf: PNG image data, 50 x 50, 8-bit/color RGBA, non-interlaced
```

Although the file has a `.pdf` extension, it is also recognized as a valid **PNG image**.

Rename the file:

```bash
mv flag2of2-final.pdf flag2of2-final.png
```

---

## Step 3 - Recover the First Half

The PNG contains the first part of the flag.

OCR can be used to extract the text from the image:

```bash
gocr flag2of2-final.png
```

Example output:

```text
picoCTF{f1u3
n7_
```

Combining the text extracted from the PNG with the second half shown in the PDF reconstructs the complete flag.

---

# Flag

```text
picoCTF{REDACTED}
```

---

# Why the Attack Works

A **polyglot file** is crafted so that it is valid under multiple file formats simultaneously.

In this challenge:

* PDF readers interpret the file as a PDF and reveal one part of the flag.
* Image-processing tools recognize the same file as a PNG image, revealing the remaining part.

By examining the file in both formats, the complete flag can be recovered.

---

# Key Takeaways

* A file extension does not always represent the file's true format.
* The `file` command identifies files based on their internal signatures.
* Polyglot files can be interpreted correctly by multiple applications.
* Viewing a suspicious file in different ways may reveal hidden information.

---

# Tools Used

* file
* mv
* gocr
* PDF Viewer
* Linux Terminal
