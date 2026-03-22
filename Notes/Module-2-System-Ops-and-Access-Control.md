# Module 2: System Operations & Access Control

**Session Date:** March 8, 2026  
**Program Track:** Cyber Operations  
**Focus:** Permissions, CLI Efficiency, and Initial Triage

---

## 🔄 Review: The Foundation

Before diving into permissions, we reinforced the core Linux principle: **"Everything is a file."** This concept is the bedrock of system administration and security. If you can control the file, you can control the system.

---

## 🔐 Linux File Permissions (The Octal Model)

Access control in Linux is managed through a triplet of permissions: **Read (r)**, **Write (w)**, and **Execute (x)**. These are applied to the **Owner**, **Group**, and **Others**.

### The Numeric (Octal) Representation

| Permission | Value | Binary |
| :--- | :---: | :---: |
| Read (r) | 4 | 100 |
| Write (w) | 2 | 010 |
| Execute (x) | 1 | 001 |
| No Access (-) | 0 | 000 |

### Common Permission Configurations

*   **755 (`rwxr-xr-x`):** Full access for owner; read/execute for others. Standard for scripts and public directories.
*   **644 (`rw-r--r--`):** Read/write for owner; read-only for others. Ideal for configuration files.
*   **700 (`rwx------`):** Private access only. Used for sensitive scripts and SSH keys.
*   **400 (`r--------`):** Read-only for owner. Critical for protecting private keys.
*   **777 (`rwxrwxrwx`):** **CRITICAL RISK.** Everyone has full access. Never use in production.

### Advanced Access Bits

| Bit | Octal | Effect |
| :--- | :---: | :--- |
| **SUID** | 4000 | File executes with the permissions of the owner (e.g., `passwd`). |
| **SGID** | 2000 | File executes with the permissions of the group. |
| **Sticky Bit** | 1000 | Only the file owner can delete files within the directory (e.g., `/tmp`). |

---

## ⌨️ Command Line Proficiency

### Essential Navigation & Management
```bash
# Location & Listing
pwd                        # Identify current path
ls -la                     # Detailed list including hidden files
ls -ltr                    # List by time (reverse) to see newest changes

# File Operations
mkdir -p lab/red/blue      # Create nested directory trees
touch evidence.txt         # Create a new empty file
cp -r source/ backup/      # Recursive copy for directories
mv old_name new_name       # Rename or move assets
rm -rf suspicious_dir/     # Force removal (use with extreme caution)

# Content Manipulation
cat config.yaml            # Display file content
grep "API_KEY" .env        # Search for specific strings
echo "LOG: Start" > log.txt # Overwrite file with text
echo "LOG: End" >> log.txt   # Append text to file
```

### Power User: Brace Expansion
```bash
# Rapidly create complex structures
mkdir -p infra/{web,db,app}/{logs,configs,data}

# Batch file creation
touch report_{2024..2026}.md
```

---

## 🕵️ Lab: Incident Response & Flag Hunting

During the lab, we analyzed a suspicious payload that dropped three "flags" (hidden files) across the system.

### Flag 1: The `/tmp` Trap
**Discovery:** Attackers often use `/tmp` because it is world-writable.
**Method:** `ls -la /tmp` revealed an unusual `.hidden_flag.txt`.
**Lesson:** Always audit temporary directories for persistence or staging.

### Flag 2: Log Mimicry
**Discovery:** The second flag was hidden within `/var/log`, disguised as a system log.
**Method:** `ls -lt /var/log` showed a file modified exactly when the payload ran.
**Lesson:** Time-sorting logs is a powerful technique for spotting anomalies.

### Flag 3: Environment Persistence (In Progress)
**Hypothesis:** The flag might be stored in an environment variable or a hidden user config.
**Search Commands:**
```bash
printenv | grep -i "FLAG"
find /home -name ".*" -type f 2>/dev/null
ps auxww | grep -i "flag"
```

---

## 🛠️ Triage Toolkit

*   `strings <file>`: Extract ASCII text from binaries.
*   `file <file>`: Determine file type regardless of extension.
*   `find / -perm -4000`: Locate SUID binaries (potential privilege escalation vectors).
*   `grep -rnw '/' -e 'pattern'`: Search for text recursively across the entire system.

---

## 📝 Reflections & Next Steps

1.  **Permission Hardening:** Always follow the principle of least privilege (PoLP).
2.  **Automation:** Use brace expansion and scripting to reduce manual errors.
3.  **Visibility:** `ls -la` and `ps aux` are your best friends during initial triage.

**Homework:**
- [ ] Complete Bandit levels 5-12.
- [ ] Locate the final flag from the payload challenge.
- [ ] Document the `chmod` numeric vs. symbolic syntax.
