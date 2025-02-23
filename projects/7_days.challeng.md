# **7-Day Cybersecurity Challenge**

## **Day 1: Introduction to Cybersecurity & Linux Basics**
### **Objective:** Understand cybersecurity fundamentals and learn essential Linux commands.

### **Tasks:**
1. **Learn the Basics of Cybersecurity:**
   - Research the **CIA Triad (Confidentiality, Integrity, Availability)**.
   - Understand common threats like **phishing, malware, and ransomware**.
   
2. **Set Up a Cybersecurity Lab:**
   - Install **VirtualBox** or **VMware**. (If you don't have already)
   - Download and install **Kali Linux** or **Ubuntu**. ( if you don't have already)
   
3. **Practice Basic Linux Commands:**
   - Open a terminal and run:
     ```bash
     ls  # List files
     cd /home  # Change directory
     pwd  # Show current directory
     mkdir test_folder  # Create a folder
     rm -r test_folder  # Remove a folder
     ```
   - Learn how to use the **man command** for help (`man ls`).
   
4. **Create a Simple Bash Script:**
   - Open a text editor (`nano script.sh`) and type:
     ```bash
     #!/bin/bash
     echo "Hello, Cybersecurity World!"
     ```
   - Save the file, make it executable (`chmod +x script.sh`), and run it (`./script.sh`).

---

## **Day 2: Network Traffic Monitoring & Packet Analysis**
### **Objective:** Capture and analyze network traffic for suspicious activity.

### **Tasks:**
1. **Install Wireshark:**
   - Run: `sudo apt install wireshark -y` (Linux) or download from [Wireshark's website](https://www.wireshark.org/).
   
2. **Capture Network Traffic:**
   - Open Wireshark, select an active network interface, and start capturing.
   - Filter packets using:
     - `http` (for web traffic)
     - `dns` (for domain name lookups)
     - `icmp` (for ping requests)
   
3. **Analyze a PCAP File:**
   - Open a sample **PCAP file** (Download from [https://wiki.wireshark.org/uploads/27707187aeb30df68e70c8fb9d614981/http.cap)).
   - Look for credentials sent in plaintext (**HTTP, FTP, Telnet** traffic).
   
4. **Identify Suspicious Activity:**
   - Look for large data transfers.
   - Check for multiple failed login attempts.
   - Find unexpected IP addresses communicating with your machine.

---

## **Day 3: Basic Vulnerability Scanning with Nmap**
### **Objective:** Discover open ports and potential vulnerabilities on a system.

### **Tasks:**
1. **Install Nmap:**
   - Run: `sudo apt install nmap -y`
   
2. **Scan Your Local Network:**
   - Discover live hosts: `nmap -sn 192.168.1.0/24`
   - Scan for open ports: `nmap -sS <target IP>`
   - Identify service versions: `nmap -sV <target IP>`
   
3. **Analyze Open Ports:**
   - Check for common ports:
     - `22` (SSH)
     - `80` (HTTP)
     - `443` (HTTPS)
     
4. **Use Zenmap (GUI Version of Nmap)**
   - Install: `sudo apt install zenmap -y`
   - Run scans and visualize network topology.

---

## **Day 4: Web Security Basics & SQL Injection**
### **Objective:** Understand and exploit SQL Injection vulnerabilities.

### **Tasks:**
1. **Set Up a Vulnerable Web Application:**
   - Install **DVWA (Damn Vulnerable Web App)** using Docker:
     ```bash
     docker run --rm -it -p 80:80 vulnerables/web-dvwa
     ```
   
2. **Perform Basic SQL Injection:**
   - Open DVWA and log in (user: `admin`, password: `password`).
   - Set security to **low**.
   - In the login field, try:
     ```sql
     ' OR '1'='1' --
     ```
   - Observe if you bypass authentication.
   
3. **Use SQLMap for Automated Attacks:**
   - Run: `sqlmap -u "http://target-site.com/login.php?id=1" --dbs`

---

## **Day 5: Password Cracking & Hash Analysis**
### **Objective:** Learn how weak passwords can be cracked.

### **Tasks:**
1. **Generate a Hash of a Password:**
   - Run:
     ```bash
     echo -n "password123" | md5sum
     ```
   
2. **Crack Hashes Using Hashcat:**
   - Install: `sudo apt install hashcat -y`
   - Crack an MD5 hash using a wordlist:
     ```bash
     hashcat -m 0 -a 0 hash.txt rockyou.txt
     ```
   
3. **Use Hydra for Brute-Forcing:**
   - Attack an SSH login page:
     ```bash
     hydra -l admin -P rockyou.txt ssh://192.168.1.10
     ```

---

## **Day 6: Introduction to Digital Forensics**
### **Objective:** Learn how to recover deleted files and analyze evidence.

### **Tasks:**
1. **Install Autopsy:**
   - Run: `sudo apt install autopsy -y`
   - Open: `autopsy` in a browser.
   
2. **Analyze a USB Drive Image:**
   - Load a forensic disk image (.dd file).
   - Recover deleted files.
   
3. **Extract Metadata from an Image File:**
   - Run:
     ```bash
     exiftool image.jpg
     ```

---

## **Day 7: Simple Capture The Flag (CTF) Challenge**
### **Objective:** Solve beginner-friendly security challenges.

### **Tasks:**
1. **Join TryHackMe or PicoCTF:**
   - Sign up at [PicoCTF](https://picoctf.org).
   
2. **Complete this Basic Challenge:**
   - https://play.picoctf.org/practice/challenge/147?page=4

3. **Write a CTF Write-Up:**
   - Document the steps taken to solve each challenge.
   
---

## **Final Notes:**
- Keep track of what you learn each day.
- Ask questions and explore new topics.
- Cybersecurity is all about practice—keep hacking.

