# CTF Write-up: System Enumeration & Flag Discovery

**Challenge Name:** Payload Analysis #01  
**Date:** March 8, 2026  
**Category:** Linux Forensics / Enumeration  
**Difficulty:** Level 1 (Beginner)  
**Progress:** 2 / 3 Flags Captured

---

## 🎯 Objective

The goal of this lab was to analyze a provided binary payload, execute it in a controlled environment, and locate three hidden flags dropped by the process across the filesystem.

---

## 🔍 Initial Reconnaissance

Before execution, I performed basic static analysis on the `payload` binary to understand its nature.

### Static Analysis Commands:
```bash
# Identify file type and architecture
file ./payload

# Extract human-readable strings
strings ./payload | head -n 20

# Check current permissions
ls -l ./payload

# Grant execution rights if necessary
chmod +x ./payload
```

### Execution:
```bash
./payload
```

---

## 🚩 Flag 1: The Temporary Staging Area

**Status:** ✅ Captured  
**Location:** `/tmp/admin_data_leak.txt`  
**Technique:** Directory Listing & Time Sorting

### Analysis:
The `/tmp` directory is a common target for attackers because it is world-writable and often overlooked by casual users. I checked for any new files created immediately after running the payload.

### Discovery:
```bash
# List all files in /tmp, sorted by modification time
ls -lt /tmp | head

# Found: admin_data_leak.txt
cat /tmp/admin_data_leak.txt
```

**Flag:** `FLAG{SYSTEM_TMP_EXPOSED}`

---

## 🚩 Flag 2: Log File Obfuscation

**Status:** ✅ Captured  
**Location:** `/var/log/sys_audit.log`  
**Technique:** Recursive Grep & Log Analysis

### Analysis:
Advanced malware often hides its presence within legitimate system directories like `/var/log`. By mimicking the naming convention of standard logs, it can evade detection.

### Discovery:
```bash
# Search recursively for the flag pattern within the log directory
grep -rn "FLAG{" /var/log/ 2>/dev/null

# Alternative: Sort logs by time to see the most recent additions
ls -lt /var/log | grep "Mar  8"
```

**Flag:** `FLAG{LOG_SPOOFING_DETECTED}`

---

## 🚩 Flag 3: Deep Persistence (In Progress)

**Status:** ⏳ Pending  
**Current Hypothesis:** The final flag is likely hidden in a less obvious location, possibly involving environment variables, hidden user configurations, or encoded within the binary itself.

### Investigation Plan:
1.  **Environment Audit:** `printenv | grep -i "FLAG"`
2.  **Hidden File Search:** `find ~ -name ".*" -ls`
3.  **SUID Enumeration:** `find / -perm -4000 -type f 2>/dev/null`
4.  **Binary Decoding:** `strings payload | grep "==" | base64 -d`
5.  **Process Inspection:** `ps auxww | grep "payload"`

---

## 🛠️ Tool Summary

| Tool | Function |
| :--- | :--- |
| `ls -lt` | Sort files by time to identify recent changes. |
| `grep -rn` | Recursive search for patterns with line numbers. |
| `strings` | Extract ASCII text from binary files. |
| `find` | Locate files based on specific attributes (permissions, owner). |
| `file` | Determine the true nature of a file. |

---

## 💡 Lessons Learned

*   **Speed is Essential:** Checking `/tmp` and `/var/log` immediately after a suspicious event is a standard IR (Incident Response) procedure.
*   **Context Matters:** A file's location often tells you more about the attacker's intent than the file's content.
*   **Tool Agnostic:** During this lab, I had to rely on basic CLI tools. Understanding the underlying system is more important than having a fancy GUI.

---

> *"The best place to hide a leaf is in a forest. The best place to hide a flag is in a log file."*
