## 🛡️ Bash Scripting Program for OS Hardening (Beginner to Advanced)

---

### 📘 Module 1: Introduction to Bash Scripting (Beginner)

**Topics Covered:**

* What is Bash?
* Writing and executing your first script
* Variables, conditionals, loops
* Permissions and running scripts safely

**Practice Scripts:**

1. **Hello World Script**

   ```bash
   #!/bin/bash
   echo "Hello, welcome to OS Hardening!"
   ```

2. **User Info Script**

   ```bash
   #!/bin/bash
   echo "Current User: $(whoami)"
   echo "Home Directory: $HOME"
   ```

3. **File Permission Checker**

   ```bash
   #!/bin/bash
   echo "Enter file name:"
   read filename
   ls -l $filename
   ```

---

### 📙 Module 2: Intermediate Bash + Basic OS Hardening

**Topics Covered:**

* File permissions and ownership
* Password policy enforcement
* User and group management
* Basic auditing

**Practice Scripts:**

1. **Set Strong Password Policy**

   ```bash
   #!/bin/bash
   echo "Setting strong password policy..."
   sed -i 's/^PASS_MAX_DAYS.*/PASS_MAX_DAYS   90/' /etc/login.defs
   sed -i 's/^PASS_MIN_DAYS.*/PASS_MIN_DAYS   10/' /etc/login.defs
   sed -i 's/^PASS_WARN_AGE.*/PASS_WARN_AGE   7/' /etc/login.defs
   echo "Password policy updated."
   ```

2. **Disable Unused Users**

   ```bash
   #!/bin/bash
   echo "Disabling guest account..."
   usermod -L guest
   echo "Guest account locked."
   ```

3. **Audit Critical Files**

   ```bash
   #!/bin/bash
   files=("/etc/passwd" "/etc/shadow" "/etc/hosts")
   for file in "${files[@]}"; do
       echo "Checking $file..."
       ls -l $file
   done
   ```

---

### 📕 Module 3: Advanced OS Hardening with Bash

**Topics Covered:**

* Automating system updates
* Configuring firewall with `ufw` or `iptables`
* Securing SSH
* Disabling dangerous services

**Practice Scripts:**

1. **Secure SSH Configuration**

   ```bash
   #!/bin/bash
   echo "Securing SSH..."
   sed -i 's/^#Port 22/Port 2222/' /etc/ssh/sshd_config
   sed -i 's/^#PermitRootLogin.*/PermitRootLogin no/' /etc/ssh/sshd_config
   sed -i 's/^#PasswordAuthentication.*/PasswordAuthentication no/' /etc/ssh/sshd_config
   systemctl restart sshd
   echo "SSH secured."
   ```

2. **Disable Unnecessary Services**

   ```bash
   #!/bin/bash
   services=("telnet" "ftp" "rsh" "rlogin")
   for s in "${services[@]}"; do
       systemctl disable $s 2>/dev/null
       systemctl stop $s 2>/dev/null
       echo "Disabled $s"
   done
   ```

3. **Enable and Configure Firewall (UFW)**

   ```bash
   #!/bin/bash
   echo "Configuring UFW Firewall..."
   ufw default deny incoming
   ufw default allow outgoing
   ufw allow 2222/tcp  # SSH on custom port
   ufw enable
   echo "Firewall enabled and configured."
   ```

---

### 📗 Module 4: Pro-Level Projects & Automation

**Topics Covered:**

* Creating compliance check scripts
* CIS Benchmark checks (basic)
* Log monitoring automation
* Hardened system audit reports

**Project Ideas:**

1. **Automated Security Checklist Script**

   * Check kernel version
   * Check for open ports
   * Check password aging policy
   * Check firewall status
   * Output report to a file

2. **System Update & Log Cleanup**

   ```bash
   #!/bin/bash
   echo "Updating system and cleaning logs..."
   apt update && apt upgrade -y
   journalctl --vacuum-time=7d
   echo "System updated and logs cleaned."
   ```

3. **Create Weekly Cron Job for Hardening Script**

   ```bash
   #!/bin/bash
   cp hardening-script.sh /usr/local/bin/
   echo "0 2 * * 0 root /usr/local/bin/hardening-script.sh" > /etc/cron.d/weekly-hardening
   echo "Scheduled hardening script."
   ```




* Sets password policy
* Disables root login
* Updates the system
* Enables firewall
* Scans for open ports
* Creates a security audit report

