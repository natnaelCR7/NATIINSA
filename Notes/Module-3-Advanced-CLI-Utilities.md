# Module 3: Advanced CLI Utilities & Privilege Escalation

**Session Date:** March 14, 2026  
**Program Track:** Cyber Operations  
**Focus:** Enumeration, Power Tools, and Escalation Vectors

---

## 🔄 Knowledge Check

We began by reviewing the Linux FHS and the octal permission system. A solid grasp of where files live and who can access them is the prerequisite for advanced system analysis.

---

## 📈 Privilege Escalation (PrivEsc) Fundamentals

Privilege escalation is the act of exploiting a bug, design flaw, or configuration error in an operating system or software application to gain unauthorized access to resources that are normally protected from an application or user.

### Escalation Vectors

| Vector | Description | Command to Audit |
| :--- | :--- | :--- |
| **Vertical** | Moving from a low-privilege user to `root`. | `sudo -l` |
| **Horizontal** | Gaining access to another user's account at the same level. | `ls -la /home` |
| **SUID/SGID** | Binaries that run with the owner's privileges. | `find / -perm -4000 -type f 2>/dev/null` |
| **Cron Jobs** | Scheduled tasks running as a high-privilege user. | `cat /etc/crontab` |
| **Writable Scripts** | Scripts run by root that can be modified by users. | `find / -writable -type f 2>/dev/null` |

---

## 🛠️ The Power User's Toolkit

### 1. `sudo` (Superuser Do)
The primary tool for administrative tasks.
*   `sudo -l`: **Crucial.** Lists the allowed (and forbidden) commands for the invoking user.
*   `sudo -u <user> <command>`: Run a command as a specific user.
*   **Security Risk:** `NOPASSWD` entries in `/etc/sudoers` are a common path to instant root access.

### 2. `grep` (Pattern Matching)
The "search engine" for file contents.
*   `grep -i "secret" config.php`: Case-insensitive search.
*   `grep -r "password" /var/www`: Recursive search through a directory.
*   `grep -v "nologin" /etc/passwd`: Invert match (show users with a shell).
*   `ps aux | grep "python"`: Filter process list for specific applications.

### 3. `find` (Filesystem Discovery)
Locate files based on metadata rather than content.
*   `find . -name "*.conf"`: Find by extension.
*   `find / -user root -perm -4000 2>/dev/null`: Locate SUID binaries owned by root.
*   `find /var/log -mtime -1`: Find logs modified in the last 24 hours.
*   `find / -size +50M`: Identify large files (potential data exfiltration staging).

### 4. `strings` (Binary Analysis)
Extract human-readable text from non-text files.
*   `strings suspicious_binary | grep -i "http"`: Look for C2 callback URLs.
*   `strings -n 10 <file>`: Only show strings longer than 10 characters to reduce noise.

---

## 📝 Terminal-Based Text Editing

In a headless server environment, proficiency in a terminal editor is non-negotiable.

### **Vim (The Professional Standard)**
*   **Modes:** Normal (navigation/commands) and Insert (typing).
*   **Commands:**
    *   `i`: Enter Insert mode.
    *   `Esc`: Return to Normal mode.
    *   `:wq`: Save and exit.
    *   `:q!`: Exit without saving.
    *   `dd`: Delete a line.
    *   `/pattern`: Search for text.

### **Nano (The Quick Alternative)**
*   **Usage:** `nano <file>`
*   **Commands:** `Ctrl+O` (Save), `Ctrl+X` (Exit).

---

## 🚩 Practical Application: Enumeration

During the lab, we practiced "Living off the Land" (LotL) techniques—using built-in tools to gather intelligence.

**Scenario:** You have initial access as user `web-data`.
1.  **Who am I?** `id`, `whoami`
2.  **What can I do?** `sudo -l`
3.  **What's on the system?** `ls -la /opt`, `ls -la /tmp`
4.  **Are there SUID files?** `find / -perm -4000 -type f 2>/dev/null`
5.  **Any interesting strings?** `strings /usr/local/bin/custom_app`

---

## 💡 Key Takeaways

*   **Enumeration is Key:** 90% of privilege escalation is thorough enumeration.
*   **Tool Synergy:** Combine `find`, `grep`, and `strings` using pipes (`|`) for powerful analysis.
*   **Vim Mastery:** Being comfortable in Vim allows you to modify configs and write scripts on any Linux system.

---

## 📅 Action Items

- [ ] Audit the `/etc/sudoers` file on a test machine.
- [ ] Practice finding and decoding Base64 strings using `strings` and `base64 -d`.
- [ ] Complete the "Vim Adventures" or a similar tutorial to build muscle memory.
- [ ] Push Module 3 notes to the repository.
