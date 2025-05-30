## 🛡 Bash Projects for OS Hardening (With Briefs & Objectives)



###  Beginner-Level Projects



#### **1. User Account Audit Script**

**Goal**: List all user accounts, show if they are active, last login, and UID.

```bash
# Sample output: username | UID | Last Login | Status
```

**Learning Objectives**:

* Using `/etc/passwd`, `lastlog`, `id`
* String parsing with `cut`, `awk`

---

#### **2. Permission Fixer Script**

**Goal**: Scan home directories and fix insecure permissions (e.g., 777 files).

```bash
# Recursively find and fix permissions
```

**Learning Objectives**:

* `find`, `chmod`, `stat`
* Working with file attributes

---

#### **3. Service Status Dashboard**

**Goal**: Check and display status of key services: SSH, firewall, cron, etc.

```bash
# Output:
# SSH: active
# UFW: inactive
# CRON: active
```

**Learning Objectives**:

* `systemctl` usage
* Conditional logic and output formatting

---

### ⚙️ Intermediate-Level Projects

---

#### **4. Secure SSH Configuration Tool**

**Goal**: Automatically harden SSH by editing `/etc/ssh/sshd_config`.

**Tasks**:

* Change default port
* Disable root login
* Disable password auth
* Restart service

**Learning Objectives**:

* `sed` editing
* Backing up configs
* Service management

---

#### **5. Password Policy Enforcer**

**Goal**: Enforce password complexity and aging policies.

**Tasks**:

* Modify `/etc/login.defs`
* Edit `/etc/pam.d/common-password` for complexity
* Notify users via echo/log file

---

#### **6. Firewall Rules Setup Tool**

**Goal**: Automate firewall setup using `ufw` or `iptables`.

**Tasks**:

* Deny all incoming by default
* Allow specific ports
* Save rules and enable

---

#### **7. Unused Services Cleaner**

**Goal**: Detect and disable unused or insecure services (telnet, ftp, etc.)

```bash
# Detect running services and disable known insecure ones
```

---

### 🔐 Advanced-Level Projects

---

#### **8. CIS Benchmark Checker (Light Version)**

**Goal**: Create a script that checks if certain CIS hardening rules are met.

**Examples**:

* Root login disabled?
* Password expiration set?
* Unused services removed?
* Firewall enabled?

**Output**: PASS/FAIL with suggestions

---

#### **9. Weekly Audit Report Generator**

**Goal**: Run scheduled scans and save logs to `/var/log/security-audit.log`

**Tasks**:

* Check open ports (`ss`, `netstat`, `nmap`)
* Users with UID 0
* SUID/SGID files
* Print summary report

---

#### **10. System Update & Patch Script with Logging**

**Goal**: Check for updates, install them, log actions.

**Tasks**:

* `apt/yum update` with timestamped logs
* Notify via email (optional with `mail`)

---

#### **11. Automated Backup with Permissions Check**

**Goal**: Backup critical directories (`/etc`, `/home`, etc.) and check permissions.

**Tasks**:

* Use `tar` to compress
* Save backup with timestamp
* Verify backup integrity (`md5sum`)
* Restrict access to backups

---

#### **12. Hardening Cron Setup Tool**

**Goal**: Allow users to pick and schedule hardening tasks via cron.

**Tasks**:

* Menu-driven options (1: password check, 2: update, etc.)
* Save selected tasks as cron jobs
* Logs success/failure

---

## 🧩 Bonus Capstone Projects

---

### **13. All-in-One Linux Hardening Script**

Build a full-scale script that:

* Applies password and SSH policies
* Disables unused accounts and services
* Configures the firewall
* Generates a hardening log/report
* Can be reused across systems

---

### **14. Threat Detection Script (Basic EDR)**

Simulate a basic endpoint detection:

* Monitor file changes in `/etc` with `inotifywait`
* Alert if shadow/passwd is modified
* Log unusual login attempts (parse `auth.log`)

---

### **15. Compliance Report Generator**

Create a script that:

* Audits a Linux system
* Outputs a compliance report (HTML or markdown)
* Lists failed and passed checks
* Useful for security audits or patch checks


##  Skills Covered

| Level        | Topics                                                             |
| ------------ | ------------------------------------------------------------------ |
| Beginner     | Variables, loops, if-else, permissions                             |
| Intermediate | sed/awk, systemctl, crontab, networking                            |
| Advanced     | auditctl, firewalls, parsing logs, CIS rules, hardening automation |
