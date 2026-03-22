# CLI Mastery Log: Practical Exercises

**Track:** Cyber Operations  
**Program:** 2026 Cyber Talent Program  
**Objective:** Building speed and accuracy in the Linux terminal.

---

## 🛠️ Lab 1: Structural Efficiency (Brace Expansion)

**Task:** Replicate a complex directory hierarchy using a single command string.

### Target Structure:
```text
workspace/
├── offensive/
│   ├── payloads/
│   └── exploits/
└── defensive/
    ├── hardening/
    └── monitoring/
```

### Solution:
```bash
mkdir -p workspace/{offensive/{payloads,exploits},defensive/{hardening,monitoring}}
```

### Verification:
```bash
find workspace -maxdepth 2 -not -path '*/.*'
```

---

## 🛠️ Lab 2: Batch Asset Creation

**Task:** Generate a sequence of files for testing purposes using one command.

### Solution:
```bash
# Create a range of log files
touch access_log_{01..10}.txt

# Confirm creation
ls -1 access_log_*.txt
```

---

## 🛠️ Lab 3: Permission Hardening Drills

**Task:** Apply specific access controls to various file types.

### Exercises:
1.  **Private Script:** Owner has full access; no one else has any.
    `chmod 700 private_exploit.sh`
2.  **Shared Config:** Owner can edit; group/others can only read.
    `chmod 644 system_config.yaml`
3.  **Protected Key:** Read-only for the owner; no other access.
    `chmod 400 id_rsa_backup`

### Reference Table:
| Octal | Symbolic | Security Context |
| :--- | :--- | :--- |
| 755 | `rwxr-xr-x` | Standard for public binaries/scripts. |
| 644 | `rw-r--r--` | Standard for non-sensitive data files. |
| 700 | `rwx------` | Sensitive user scripts. |
| 400 | `r--------` | Critical secrets (SSH keys, API tokens). |

---

## 🛠️ Lab 4: Wargame Progress (Bandit)

**Platform:** [OverTheWire Bandit](https://overthewire.org/wargames/bandit/)

| Level | Status | Core Concept | Command |
| :--- | :---: | :--- | :--- |
| 0 | ✅ | SSH Authentication | `ssh bandit0@...` |
| 1 | ✅ | Standard Input/Output | `cat readme` |
| 2 | ✅ | Special Character Handling | `cat ./-` |
| 3 | ✅ | Whitespace Management | `cat "file name"` |
| 4 | ✅ | Hidden File Discovery | `ls -la` |
| 5 | ✅ | File Type Identification | `file ./*` |
| 6 | ⏳ | Property-Based Search | `find / -size 1033c` |

---

## 🛠️ Lab 5: Essential Command Checklist

*   **Navigation:** `pwd`, `cd`, `ls -la`, `tree`
*   **Manipulation:** `mkdir -p`, `touch`, `cp -r`, `mv`, `rm -rf`
*   **Inspection:** `cat`, `head`, `tail -f`, `less`
*   **Permissions:** `chmod`, `chown`, `umask`
*   **Search:** `grep -ri`, `find`, `locate`, `which`
*   **System:** `ps aux`, `top`, `df -h`, `free -m`

---

## 🛠️ Lab 6: Vim Proficiency

**Task:** Edit a configuration file without leaving the terminal.

### Workflow:
1.  `vim server_setup.sh`
2.  Press `i` to enter **Insert Mode**.
3.  Add: `#!/bin/bash \n echo "Initializing Security Protocols..."`
4.  Press `Esc` to return to **Normal Mode**.
5.  Type `:wq` and press `Enter` to save and exit.

---

## 🛠️ Lab 7: Version Control (Git)

**Requirement:** Maintain a consistent commit history to track learning progress.

### Standard Workflow:
```bash
# Stage all changes
git add .

# Commit with descriptive message
git commit -m "Add Module 3 notes and Stego write-up"

# Push to remote repository
git push origin main
```

### Commit Guidelines:
*   `feat:` New notes or challenges.
*   `fix:` Correcting errors in previous documentation.
*   `docs:` Updating README or meta-information.

---

## 📝 Reflections

*   **Brace Expansion** is a massive time-saver for setting up lab environments.
*   **Vim** feels slow initially, but the efficiency gains in remote server management are undeniable.
*   **Bandit** levels are excellent for reinforcing the "everything is a file" philosophy.
*   Need to focus more on **Regular Expressions (Regex)** for advanced `grep` usage.
