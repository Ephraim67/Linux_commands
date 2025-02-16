### 🚨 **Network Analysis – Web Shell (Local to Local Port Scanning Alert)** 🚨  

This scenario describes a **Local-to-Local Port Scanning** alert in a **Security Information and Event Management (SIEM)** system. It indicates that an internal private IP address is scanning another internal system, which could be a sign of **malware activity, lateral movement, or an attacker using a web shell** inside the network.  

---

## 🕵️‍♂️ **Breaking It Down: What Happened?**
1️⃣ **SIEM Alert: "Local to Local Port Scanning"**  
   - The **source IP** (internal private address) is scanning ports on another internal system.  
   - Normally, internal machines don’t scan each other unless it's for legitimate administrative tasks (e.g., vulnerability scanning by IT teams).  

2️⃣ **Possible Web Shell Activity**  
   - A **web shell** is a malicious script that attackers place on a compromised web server.  
   - The attacker might be using the web shell to execute **commands, scan the network**, and move laterally.  

3️⃣ **Why is this Suspicious?**  
   - **Normal users don’t run port scans.**  
   - A web shell often lets attackers control an internal system **remotely via HTTP/HTTPS**.  
   - If the system hosting the web shell has been compromised, it could be used to **probe other machines for vulnerabilities**.  

---

## 🔎 **Step 1: Investigate the Alert**
### 📌 **A. Identify the Source & Destination IPs**
Check **SIEM logs** to find:
- **Source IP** → The internal system initiating the scan.  
- **Destination IP** → The system being scanned.  
- **Ports Scanned** → Is the scanning targeting specific services (e.g., SSH, RDP, SMB)?  

👉 Run a **Splunk or Kibana query** to pull logs:
```spl
index=firewall OR index=network_traffic sourcetype=suricata 
| where event=="port_scan" 
| table _time, src_ip, dst_ip, dst_port, action
```
👉 If you have Zeek logs, use:
```spl
index=zeek sourcetype=conn
| where orig_ip==“attacker_internal_IP” 
| stats count by resp_ip, resp_port
```

---

### 📌 **B. Check for Web Shell Activity**
1️⃣ Look for **web server logs** on the suspected machine:  
```bash
sudo cat /var/log/apache2/access.log | grep "cmd="
sudo cat /var/log/nginx/access.log | grep "wget"
```
2️⃣ Check if any PHP, ASPX, or JSP scripts were uploaded:  
```bash
find /var/www/html -type f -mtime -2
```
3️⃣ Investigate unusual HTTP requests (e.g., `/uploads/shell.php?cmd=id`).

---

## 🔥 **Step 2: Containment & Mitigation**
✅ **1. Isolate the Affected Machine**  
- Block its network traffic using a firewall rule:
  ```bash
  sudo iptables -A INPUT -s <source_ip> -j DROP
  ```
- If it’s a Windows machine, remove it from the domain.

✅ **2. Search for Known Web Shells**  
Run `ClamAV` or `YARA` to detect web shells:
```bash
clamscan -r /var/www/html
yara -r webshell_rules.yar /var/www/html
```

✅ **3. Check for Reverse Shell Connections**  
If an attacker has a backdoor:
```bash
sudo netstat -anp | grep ":4444"
```

✅ **4. Reset Credentials**  
- If the attacker gained access through weak credentials, enforce **password resets**.

---

## 🎯 **Step 3: Implement Detection & Prevention**
### 🔍 **A. Set Up SIEM Rules for Port Scanning**
- **Rule Logic**: Detect multiple connection attempts from a single internal IP in a short time frame.  
- Example in Splunk:
```spl
index=network sourcetype=suricata
| stats count by src_ip, dst_port
| where count > 10
```
- Example in Suricata:
```yaml
alert ip any any -> any any (msg:"Possible Internal Port Scan"; threshold: type both, track by_src, count 10, seconds 60;)
```
- Configure Suricata to drop scanning traffic:
```yaml
drop ip any any -> any any (msg:"Detected Internal Port Scan";)
```

### 🔍 **B. Enable Web Shell Detection in WAF**
- If using AWS WAF, add **Web Shell Detection Rules**:
```json
{
  "Statement": {
    "ByteMatchStatement": {
      "SearchString": "cmd=",
      "FieldToMatch": {"QueryString": {}},
      "TextTransformations": [{"Type": "URL_DECODE"}]
    }
  }
}
```

---

## ✅ **Conclusion**
🚀 **This case indicates a possible attacker using a web shell to scan the network.**  
🔎 **Your goal as an analyst is to:**  
1. **Identify** the attacker and affected machine.  
2. **Contain** and remove the web shell.  
3. **Investigate** whether they gained access to other systems.  
4. **Harden security** to prevent future attacks. 
