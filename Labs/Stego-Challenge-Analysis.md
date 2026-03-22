# Steganography Challenge: Hidden Data Analysis

**Challenge Name:** Image Forensics #02  
**Date:** March 22, 2026  
**Category:** Digital Forensics / Steganography  
**Difficulty:** Intermediate  
**Progress:** 4 / 5 Flags Captured

---

## 🎯 Objective

The goal of this challenge was to analyze a single JPEG image (`challenge.jpg`) and extract five flags hidden using various steganographic and obfuscation techniques.

---

## 🔍 Initial Analysis

Before using specialized tools, I performed a basic check of the file's metadata and strings.

### Flag 1: Metadata Extraction
**Tool:** `exiftool`  
**Method:** Inspecting the file's EXIF data for hidden comments or unusual fields.

```bash
# View all metadata associated with the image
exiftool challenge.jpg
```

**Discovery:** The flag was found within the `Comment` field of the image metadata.  
**Flag:** `FLAG{ALWAYS_Check_Metadata}`

---

### Flag 2: String Analysis
**Tool:** `strings` + `grep`  
**Method:** Extracting all ASCII characters from the binary file and filtering for the flag pattern.

```bash
# Search for the "FLAG" string within the binary data
strings challenge.jpg | grep "FLAG"
```

**Discovery:** The flag was embedded as a plain-text string within the image's binary structure.  
**Flag:** `FLAG{Strings_leak_information}`

---

## 🛠️ Advanced Steganography

The remaining flags required more sophisticated tools to bypass encryption and hidden file structures.

### Flag 3: Steghide Passphrase Bruteforce
**Tool:** `stegseek` / `steghide`  
**Method:** Using a wordlist to bruteforce the passphrase of a `steghide` container.

```bash
# Bruteforce the passphrase using the rockyou.txt wordlist
stegseek challenge.jpg /usr/share/wordlists/rockyou.txt

# Passphrase identified: password123
# Extracted content saved to: challenge.jpg.out
cat challenge.jpg.out
```

**Flag:** `FLAG{steghide_protected}`

---

### Flag 4: Embedded Archive Extraction
**Tool:** `binwalk`, `dd`, `unzip`, `base64`  
**Method:** Identifying and extracting an appended ZIP archive from the end of the JPEG file.

```bash
# Identify embedded files and their offsets
binwalk challenge.jpg

# Extract the ZIP archive starting at offset 75667
dd if=challenge.jpg bs=1 skip=75667 of=extracted_data.zip

# Decompress the archive
unzip extracted_data.zip

# Decode the double-Base64 encoded string found inside
echo "Umt4QlIzdGhjSEJsYm1SbFpGOTZhWEI5Q2c9PQo=" | base64 -d | base64 -d
```

**Flag:** `FLAG{appended_zip}`

---

## 🚩 Flag 5: The Final Mystery

**Status:** ⏳ Pending  
**Current Status:** This flag remains undiscovered. It likely involves a more advanced technique such as LSB (Least Significant Bit) manipulation or a custom encoding scheme.

---

## 🛠️ Forensic Toolkit

| Tool | Purpose |
| :--- | :--- |
| `exiftool` | Metadata inspection and modification. |
| `strings` | Extraction of human-readable text from binaries. |
| `binwalk` | Signature-based file analysis and extraction. |
| `stegseek` | High-speed bruteforcing for `steghide`. |
| `base64` | Encoding and decoding of data. |
| `dd` | Low-level data copying and extraction. |

---

## 💡 Key Takeaways

1.  **Layered Defense:** Attackers often use multiple layers of obfuscation (e.g., hiding a ZIP inside a JPEG, then encoding the content in Base64).
2.  **Metadata is a Goldmine:** Never skip the `exiftool` step; it's the lowest-hanging fruit in image forensics.
3.  **Wordlists are Essential:** For password-protected steganography, the quality of your wordlist (like `rockyou.txt`) determines your success.
4.  **Binary Offsets:** Understanding how files are structured (headers and footers) allows for manual extraction using `dd`.

---

> *"The data is there; you just need to know how to look for it."*
