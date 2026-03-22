# Module 1: Introduction to Linux & System Orientation

**Session Date:** March 7, 2026  
**Program Track:** Cyber Operations  
**Duration:** Full Day Intensive (08:30 - 17:30)

---

## 📋 Program Guidelines & Workflow

To succeed in the Cyber Talent Program, the following protocols must be observed:

*   **Attendance:** Mandatory sessions every weekend (Saturday & Sunday).
*   **Documentation:** All progress, including lab notes and challenge solutions, must be synced to **GitHub**.
*   **Tools:** Preferred environments for documentation are **Obsidian** or **Notion**.
*   **Communication:** The official Telegram channel is the primary source for all announcements and updates.

---

## 🐧 Core Linux Concepts

### Understanding the Linux Ecosystem
Linux is technically a **Kernel**—the software layer that manages hardware resources. A complete operating system is referred to as a **Distribution** (or "distro"), which combines the kernel with GNU utilities and a user interface.

**Common Security Distros:**
1.  **Kali Linux:** The industry standard for penetration testing.
2.  **Parrot Security OS:** A lightweight alternative focused on privacy.
3.  **Ubuntu:** A versatile base for both servers and desktops.

### The Universal Rule
> *"Everything in Linux is treated as a file."*

This design philosophy means that whether you are interacting with a document, a hard drive (`/dev/nvme0n1`), a running process (`/proc/[pid]`), or a network socket, the interface remains consistent.

---

## 📂 The Filesystem Hierarchy Standard (FHS)

The Linux directory structure is a tree starting at the **Root** (`/`). Understanding this layout is critical for both offensive and defensive operations.

| Path | Purpose | Security Significance |
| :--- | :--- | :--- |
| `/` | Root Directory | The base of the entire system. |
| `/bin` | User Binaries | Contains essential tools like `ls`, `grep`, and `bash`. |
| `/etc` | System Configs | **High Priority:** Stores system-wide settings and password hashes. |
| `/home` | User Data | Target for data exfiltration and personal file access. |
| `/root` | Admin Home | The ultimate goal for any attacker (Superuser access). |
| `/tmp` | Temp Files | **Risk:** Often world-writable; used to hide malicious scripts. |
| `/var/log` | System Logs | **Defense:** Essential for tracking intrusions and system health. |
| `/dev` | Device Files | Interface for hardware components. |
| `/opt` | Add-on Apps | Location for third-party security tools. |

---

## ⚔️ Practical Lab: OverTheWire (Bandit)

The **Bandit** wargame is our primary tool for building command-line proficiency.

**Connection Details:**
```bash
ssh bandit0@bandit.labs.overthewire.org -p 2220
# Initial Password: bandit0
```

### Level Summary (0-4)

| Level | Objective | Key Command |
| :--- | :--- | :--- |
| 0 → 1 | Accessing file content | `cat readme` |
| 1 → 2 | Handling special characters | `cat ./-` |
| 2 → 3 | Managing filenames with spaces | `cat "filename with spaces"` |
| 3 → 4 | Locating hidden assets | `ls -la` |

---

## 💡 Summary & Reflections

*   **Distro vs. Kernel:** Always remember that Linux is the core, and the distro is the package.
*   **FHS Mastery:** Knowing where logs (`/var/log`) and configs (`/etc`) live is the first step in system defense.
*   **CLI Speed:** Speed in the terminal is a force multiplier in high-pressure security scenarios.

---

## 📅 Action Items

- [x] Deploy Kali Linux in a virtualized environment (VirtualBox/VMware).
- [x] Initialize the GitHub repository for course tracking.
- [x] Complete Bandit levels 0 through 5.
- [x] Document initial setup and push to the main branch.
