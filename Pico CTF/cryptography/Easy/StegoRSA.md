# StegoRSA

**Platform:** picoCTF/CyLab 2026    
**Category:** Cryptography    
**Difficulty:** Easy    
**Author:** Yahaya Meddy    
**[Challenge LINK](https://learn.cylabacademy.org/library/719?page=1&search=steg)**  

## Challenge Description

A message has been encrypted using RSA. The public key is gone, but someone may have accidentally exposed the private key. The goal is to recover the key and decrypt the encrypted message.

### Hints

1. Metadata can tell you more than you expect.
2. Hex can be turned back into a key file.

---

## Files Provided

* `flag.enc`
* `image.jpg`

---

## Objective

Recover the RSA private key hidden within the provided image and use it to decrypt the encrypted file.

---

## Initial Analysis

Since the hints mention metadata, the first step is to inspect the image for hidden information.

### Examine Image Metadata

```bash
exiftool image.jpg
```

Among the metadata fields, the **Comment** field contained a very long hexadecimal string:

```text
Comment : 2d2d2d2d2d424547494e2050524956415445204b45592d...
```

The beginning of the string looked suspicious because:

```text
2d2d2d2d2d
```


This suggested that the hexadecimal data might represent a PEM-formatted RSA private key.

---

## Extracting the Private Key

### Step 1: Extract the Comment Field

```bash
exiftool -Comment image.jpg
```

Save the hexadecimal output into a file:

```bash
echo "<hex_string>" > key.hex
```

### Step 2: Convert Hexadecimal to Binary Data

```bash
xxd -r -p key.hex > private_key.pem
```

The resulting file contained a valid RSA private key:

```text
-----BEGIN PRIVATE KEY-----
[REDACTED]
-----END PRIVATE KEY-----
```

---

## Decrypting the Encrypted File

With the recovered private key, the encrypted file can be decrypted using OpenSSL.

```bash
openssl pkeyutl -decrypt \
  -inkey private_key.pem \
  -in flag.enc \
  -out decrypted.txt
```

View the decrypted contents:

```bash
cat decrypted.txt
```

The plaintext message revealed the challenge flag.

---

## Flag

```text
picoCTF{REDACTED}
```

---

## Key Takeaways

* Image metadata can contain sensitive information and should always be examined during forensic and steganography challenges.
* Hexadecimal data often hides encoded files and can be reconstructed using tools such as `xxd`.
* RSA encryption relies on the secrecy of the private key; exposing the private key completely breaks the security of the system.
* Tools such as `exiftool`, `xxd`, and `openssl` are essential for solving many cryptography and forensic CTF challenges.

---

## Tools Used

* ExifTool
* xxd
* OpenSSL
* Linux Command Line
