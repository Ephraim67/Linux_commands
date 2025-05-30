## Project: Weekly Audit Report Generator

###  **Goal:**

Create a **Bash script** that:

* Runs a security audit
* Saves the results to `/var/log/security-audit.log`
* Is scheduled to run **weekly** via cron



##  Step-by-Step Guide



### **Step 1: Script Outline**

Create a file named:

```bash
sudo nano /usr/local/bin/weekly-audit.sh
```

Add the following shebang line:

```bash
#!/bin/bash
```



### **Step 2: Create Log Directory & File**

Add this to the top of the script to define the log file path:

```bash
LOGFILE="/var/log/security-audit.log"
DATE=$(date '+%Y-%m-%d %H:%M:%S')

echo "===== Security Audit Report - $DATE =====" >> "$LOGFILE"
```



### **Step 3: Check Open Ports**

Use `ss`, `netstat`, or `nmap` (choose based on availability):

```bash
echo -e "\n[Open Ports - ss]" >> "$LOGFILE"
ss -tuln >> "$LOGFILE" 2>/dev/null

# Optional: if nmap is installed
# echo -e "\n[Open Ports - nmap]" >> "$LOGFILE"
# nmap -sT -O localhost >> "$LOGFILE" 2>/dev/null
```



### **Step 4: List Users with UID 0 (Superusers)**

```bash
echo -e "\n[Users with UID 0]" >> "$LOGFILE"
awk -F: '($3 == 0) {print $1}' /etc/passwd >> "$LOGFILE"
```

---

### **Step 5: Find SUID and SGID Files**

```bash
echo -e "\n[SUID Files]" >> "$LOGFILE"
find / -perm -4000 -type f -exec ls -lh {} \; 2>/dev/null | awk '{print $NF}' >> "$LOGFILE"

echo -e "\n[SGID Files]" >> "$LOGFILE"
find / -perm -2000 -type f -exec ls -lh {} \; 2>/dev/null | awk '{print $NF}' >> "$LOGFILE"
```

---

### **Step 6: Add Summary and End**

```bash
echo -e "\n[Summary]" >> "$LOGFILE"
echo "Audit completed on $DATE" >> "$LOGFILE"
echo "==========================================" >> "$LOGFILE"
```

---

### **Step 7: Make the Script Executable**

```bash
sudo chmod +x /usr/local/bin/weekly-audit.sh
```

---

## 📆 Step 8: Schedule the Script with Cron (Weekly)

Open the root user's crontab:

```bash
sudo crontab -e
```

Add the following line to run every Sunday at 2 AM:

```bash
0 2 * * 0 /usr/local/bin/weekly-audit.sh
```

---

## 🔐 Step 9: Set Permissions (Secure the Log)

Make sure only root can read the log:

```bash
sudo touch /var/log/security-audit.log
sudo chown root:root /var/log/security-audit.log
sudo chmod 600 /var/log/security-audit.log
```

---

## ✅ Optional: Email Report (Bonus)

If your system has `mail` installed and configured:

```bash
mail -s "Weekly Security Audit Report" you@example.com < /var/log/security-audit.log
```

---

## 📁 Full Script Summary

Here's the complete version:

```bash
#!/bin/bash
LOGFILE="/var/log/security-audit.log"
DATE=$(date '+%Y-%m-%d %H:%M:%S')

echo "===== Security Audit Report - $DATE =====" >> "$LOGFILE"

echo -e "\n[Open Ports - ss]" >> "$LOGFILE"
ss -tuln >> "$LOGFILE" 2>/dev/null

echo -e "\n[Users with UID 0]" >> "$LOGFILE"
awk -F: '($3 == 0) {print $1}' /etc/passwd >> "$LOGFILE"

echo -e "\n[SUID Files]" >> "$LOGFILE"
find / -perm -4000 -type f -exec ls -lh {} \; 2>/dev/null | awk '{print $NF}' >> "$LOGFILE"

echo -e "\n[SGID Files]" >> "$LOGFILE"
find / -perm -2000 -type f -exec ls -lh {} \; 2>/dev/null | awk '{print $NF}' >> "$LOGFILE"

echo -e "\n[Summary]" >> "$LOGFILE"
echo "Audit completed on $DATE" >> "$LOGFILE"
echo "==========================================" >> "$LOGFILE"
```


