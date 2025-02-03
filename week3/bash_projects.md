## **1. Log File Analysis for Suspicious Activity**
### **Description:**  
Monitor system logs for failed login attempts, unauthorized access, or unusual activity.

### **Key Skills:**
- Log parsing
- Pattern matching (`grep`, `awk`, `sed`)
- Alerting via email or logging mechanisms

### **Script Example:**
```bash
#!/bin/bash

LOG_FILE="/var/log/auth.log"  # Change this for different distros
ALERT_THRESHOLD=5
IP_LIST="/tmp/suspicious_ips.txt"

echo "Suspicious login attempts detected:" > $IP_LIST

# Extract failed login attempts
grep "Failed password" $LOG_FILE | awk '{print $(NF-3)}' | sort | uniq -c | sort -nr | while read count ip; do
    if [ "$count" -gt "$ALERT_THRESHOLD" ]; then
        echo "IP: $ip - Attempts: $count" >> $IP_LIST
    fi
done

# Send an alert if suspicious activity is found
if [ -s $IP_LIST ]; then
    cat $IP_LIST | mail -s "Suspicious Login Attempts Detected" security@example.com
fi
```
**Enhancements:**
- Block suspicious IPs using `iptables` or `fail2ban`.
- Use a cron job to automate execution.

---

## **2. Port Scanning and Network Reconnaissance**
### **Description:**  
Scan open ports on a target system and identify running services.

### **Key Skills:**
- Network scanning (`netcat`, `nmap`)
- Process automation
- Parsing command output

### **Script Example:**
```bash
#!/bin/bash

TARGET="192.168.1.1"  # Change this to your target IP
PORTS=(22 80 443 3389)

echo "Scanning open ports on $TARGET..."

for port in "${PORTS[@]}"; do
    nc -z -v -w1 $TARGET $port &> /dev/null
    if [ $? -eq 0 ]; then
        echo "Port $port is open."
    else
        echo "Port $port is closed."
    fi
done
```
**Enhancements:**
- Integrate `nmap` for advanced scanning.
- Store results in a log file for later analysis.

---

## **3. Automated Malware Detection Using Hash Comparison**
### **Description:**  
Monitor system files and compare their hashes against a database of known good hashes to detect unauthorized changes.

### **Key Skills:**
- File integrity monitoring
- Cryptographic hashing (`sha256sum`, `md5sum`)
- Automation with cron jobs

### **Script Example:**
```bash
#!/bin/bash

BASE_DIR="/home/user/monitor"
HASH_FILE="/home/user/hashes.txt"

echo "Generating new file hashes..."
find $BASE_DIR -type f -exec sha256sum {} \; > /tmp/new_hashes.txt

echo "Comparing with previous hashes..."
diff /tmp/new_hashes.txt $HASH_FILE > /tmp/hash_diff.txt

if [ -s /tmp/hash_diff.txt ]; then
    echo "WARNING: File changes detected!"
    cat /tmp/hash_diff.txt | mail -s "File Integrity Alert" security@example.com
    cp /tmp/new_hashes.txt $HASH_FILE
else
    echo "No changes detected."
fi
```
**Enhancements:**
- Store historical hashes for deeper analysis.
- Integrate with intrusion detection systems like `OSSEC`.

---

## **4. Automating Firewall Rules Management**
### **Description:**  
A script to automatically update firewall rules based on a blocklist of malicious IPs.

### **Key Skills:**
- `iptables` firewall management
- Automating security rules
- Fetching external threat intelligence feeds

### **Script Example:**
```bash
#!/bin/bash

BLOCKLIST_URL="https://example.com/malicious_ips.txt"
BLOCKLIST_FILE="/tmp/malicious_ips.txt"

# Download the latest malicious IP list
wget -q $BLOCKLIST_URL -O $BLOCKLIST_FILE

echo "Blocking malicious IPs..."
while IFS= read -r ip; do
    sudo iptables -A INPUT -s $ip -j DROP
done < $BLOCKLIST_FILE

echo "Firewall updated successfully."
```
**Enhancements:**
- Use `ufw` instead of `iptables` for simpler firewall management.
- Schedule it with cron jobs to run periodically.

---

## **5. Brute-Force Detection and Auto-Blocking**
### **Description:**  
Monitor failed login attempts and automatically block IPs exceeding a threshold.

### **Key Skills:**
- Log analysis
- Dynamic firewall management
- Automating responses to security threats

### **Script Example:**
```bash
#!/bin/bash

LOG_FILE="/var/log/auth.log"
THRESHOLD=5
BLOCKLIST="/tmp/blocked_ips.txt"

echo "Checking for brute-force attacks..."
grep "Failed password" $LOG_FILE | awk '{print $(NF-3)}' | sort | uniq -c | while read count ip; do
    if [ "$count" -gt "$THRESHOLD" ]; then
        echo "Blocking IP: $ip"
        sudo iptables -A INPUT -s $ip -j DROP
        echo $ip >> $BLOCKLIST
    fi
done
```
**Enhancements:**
- Automate execution with cron.
- Use `fail2ban` for more advanced protection.

---

## **6. USB Device Detection and Alerting**
### **Description:**  
Monitor unauthorized USB devices being plugged into the system.

### **Key Skills:**
- `udevadm` and `dmesg` monitoring
- USB security
- Alerting mechanisms

### **Script Example:**
```bash
#!/bin/bash

LOG_FILE="/var/log/usb.log"

echo "Monitoring USB devices..."
udevadm monitor --subsystem-match=usb | while read line; do
    echo "$line" >> $LOG_FILE
    echo "Unauthorized USB device detected: $line" | mail -s "USB Alert" security@example.com
done
```
**Enhancements:**
- Restrict USB access using `udev` rules.
- Implement logging for forensic analysis.

---

## **7. Password Strength Checker**
### **Description:**  
A script to check password strength by detecting weak patterns.

### **Key Skills:**
- Regular expressions (`grep`, `awk`)
- Password security best practices

### **Script Example:**
```bash
#!/bin/bash

echo "Enter a password to check: "
read -s PASSWORD

if [[ ${#PASSWORD} -lt 8 ]]; then
    echo "Weak password: Too short!"
elif ! [[ "$PASSWORD" =~ [A-Z] ]]; then
    echo "Weak password: No uppercase letters!"
elif ! [[ "$PASSWORD" =~ [0-9] ]]; then
    echo "Weak password: No numbers!"
elif ! [[ "$PASSWORD" =~ [\@\#\$\%\&\*] ]]; then
    echo "Weak password: No special characters!"
else
    echo "Strong password!"
fi
```
**Enhancements:**
- Implement a dictionary attack check.
- Integrate with password managers.

---
## **8. SSH Login Monitoring and Auto-Blocking**
### **Description:**  
Monitor SSH logins and block unauthorized access attempts in real-time.

### **Key Skills:**
- Log monitoring
- SSH security
- `iptables` for firewall management

### **Script Example:**
```bash
#!/bin/bash

LOG_FILE="/var/log/auth.log"
BLOCKLIST="/tmp/blocked_ssh_ips.txt"
THRESHOLD=3

echo "Monitoring SSH login attempts..."

# Check for repeated failed login attempts
grep "Failed password" $LOG_FILE | awk '{print $(NF-3)}' | sort | uniq -c | while read count ip; do
    if [ "$count" -gt "$THRESHOLD" ]; then
        echo "Blocking SSH attacker IP: $ip"
        sudo iptables -A INPUT -s $ip -j DROP
        echo $ip >> $BLOCKLIST
    fi
done
```
**Enhancements:**
- Send an email notification when an attack is detected.
- Use `fail2ban` for a more sophisticated solution.

---

## **9. Automated Vulnerability Scanner (Using Nmap)**
### **Description:**  
Perform vulnerability scans on a target system and generate reports.

### **Key Skills:**
- Network security
- Nmap automation
- Report generation

### **Script Example:**
```bash
#!/bin/bash

TARGET="192.168.1.1"
REPORT="/tmp/vuln_report.txt"

echo "Scanning $TARGET for vulnerabilities..."
nmap -sV --script=vuln $TARGET > $REPORT

echo "Scan complete. Results saved to $REPORT."
```
**Enhancements:**
- Integrate with `Nikto` for web vulnerability scanning.
- Schedule scans with a cron job.

---

## **10. Data Exfiltration Detection (Large File Transfers)**
### **Description:**  
Detect unusual outbound data transfers and alert the security team.

### **Key Skills:**
- Network traffic monitoring
- Process tracking (`lsof`, `netstat`)
- Alerting via email or logs

### **Script Example:**
```bash
#!/bin/bash

THRESHOLD=500000  # 500MB in KB
LOG_FILE="/var/log/data_exfiltration.log"

echo "Monitoring large data transfers..."

lsof -i -n | grep -E "(ESTABLISHED|SYN_SENT)" | awk '{print $9, $10}' | while read connection; do
    SIZE=$(du -k "$connection" | cut -f1)
    if [ "$SIZE" -gt "$THRESHOLD" ]; then
        echo "ALERT: Large data transfer detected: $connection ($SIZE KB)" >> $LOG_FILE
        mail -s "Data Exfiltration Alert" security@example.com < $LOG_FILE
    fi
done
```
**Enhancements:**
- Monitor specific ports (e.g., FTP, SCP, HTTP).
- Use a SIEM solution like Splunk for better tracking.

---

## **11. File System Integrity Monitoring**
### **Description:**  
Detect changes in critical system files to prevent rootkits and unauthorized modifications.

### **Key Skills:**
- File integrity monitoring (`sha256sum`)
- System forensics
- Report generation

### **Script Example:**
```bash
#!/bin/bash

WATCH_DIR="/etc/"
HASH_FILE="/tmp/system_hashes.txt"

echo "Generating baseline file hashes..."
find $WATCH_DIR -type f -exec sha256sum {} \; > $HASH_FILE

echo "Monitoring for changes..."
while true; do
    find $WATCH_DIR -type f -exec sha256sum {} \; > /tmp/current_hashes.txt
    diff $HASH_FILE /tmp/current_hashes.txt > /tmp/hash_diff.txt
    if [ -s /tmp/hash_diff.txt ]; then
        echo "WARNING: System file changes detected!"
        mail -s "File Integrity Alert" security@example.com < /tmp/hash_diff.txt
        cp /tmp/current_hashes.txt $HASH_FILE
    fi
    sleep 300  # Check every 5 minutes
done
```
**Enhancements:**
- Monitor `/boot`, `/var`, and other critical directories.
- Use `tripwire` or `AIDE` for enterprise-level monitoring.

---

## **12. Honeypot for Detecting Intrusions**
### **Description:**  
Set up a fake vulnerable service to trap attackers.

### **Key Skills:**
- Honeypots (`netcat`, `cowrie`)
- Network security monitoring
- Logging attacks

### **Script Example:**
```bash
#!/bin/bash

HONEYPORT=2222
LOG_FILE="/var/log/honeypot.log"

echo "Starting honeypot on port $HONEYPORT..."
nc -lvp $HONEYPORT | tee -a $LOG_FILE
```
**Enhancements:**
- Deploy `cowrie` for SSH honeypots.
- Send real-time alerts to a SIEM.

---

## **13. Wireless Network Security Audit**
### **Description:**  
Scan for unauthorized Wi-Fi access points and alert admins.

### **Key Skills:**
- Wireless security (`iwlist`)
- MAC address filtering
- Intrusion detection

### **Script Example:**
```bash
#!/bin/bash

KNOWN_MACS=("00:1A:2B:3C:4D:5E" "11:22:33:44:55:66")  # Trusted devices
ALERT_FILE="/tmp/rogue_devices.txt"

echo "Scanning for unauthorized Wi-Fi devices..."
iwlist wlan0 scan | grep -o -E "([0-9A-Fa-f]{2}:){5}[0-9A-Fa-f]{2}" | while read mac; do
    if [[ ! " ${KNOWN_MACS[@]} " =~ " $mac " ]]; then
        echo "Unauthorized device detected: $mac" >> $ALERT_FILE
        mail -s "Rogue Wi-Fi Device Alert" security@example.com < $ALERT_FILE
    fi
done
```
**Enhancements:**
- Integrate with `airmon-ng` for deeper scans.
- Automatically deauthenticate unauthorized devices.

---

## **14. Automated OS Hardening Script**
### **Description:**  
Apply security best practices to a Linux system.

### **Key Skills:**
- System hardening (`sysctl`, `ufw`, `iptables`)
- Security automation

### **Script Example:**
```bash
#!/bin/bash

echo "Applying security configurations..."

# Disable root SSH login
sed -i 's/PermitRootLogin yes/PermitRootLogin no/' /etc/ssh/sshd_config
systemctl restart sshd

# Enable firewall
ufw default deny incoming
ufw default allow outgoing
ufw allow ssh
ufw enable

# Restrict cron jobs
echo "ALL ALL=(ALL) NOPASSWD: /usr/sbin/cron" > /etc/sudoers.d/cron

echo "System hardening completed."
```
**Enhancements:**
- Configure `AppArmor` or `SELinux`.
- Disable USB ports for security.

---

## **15. Hidden Process Detection**
### **Description:**  
Find malicious processes that are trying to hide from `ps` or `top`.

### **Key Skills:**
- Process monitoring (`ps`, `lsof`)
- Rootkit detection

### **Script Example:**
```bash
#!/bin/bash

echo "Checking for hidden processes..."

ps aux | awk '{print $2}' > /tmp/ps_list.txt
ls /proc | grep -E '^[0-9]+$' > /tmp/proc_list.txt

diff /tmp/ps_list.txt /tmp/proc_list.txt | grep '>' | awk '{print $2}' | while read pid; do
    echo "Hidden process detected: PID $pid"
done
```
**Enhancements:**
- Integrate with `chkrootkit` or `rkhunter`.
- Automate with cron jobs.

---
