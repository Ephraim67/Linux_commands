John the Ripper (JtR) is a powerful open-source password cracking tool used by security professionals, ethical hackers, and penetration testers. This course will cover everything from basic usage to advanced techniques.

---

# **Course: Cracking Password Hashes with John the Ripper**

## **Module 1: Introduction to John the Ripper**
### **1.1 What is John the Ripper?**
- Overview of John the Ripper (JtR)
- Use cases: Ethical hacking, pentesting, security audits
- Supported operating systems (Linux, macOS, Windows)

### **1.2 Installation**
- Installing on Linux:
  ```bash
  sudo apt update && sudo apt install john
  ```
  or
  ```bash
  git clone https://github.com/openwall/john.git
  cd john/src && ./configure && make -s clean && make -sj4
  ```
- Installing on Windows (using Cygwin)
- Installing on macOS (via Homebrew):
  ```bash
  brew install john
  ```
- Verifying installation:
  ```bash
  john --help
  ```

---

## **Module 2: Understanding Hashes**
### **2.1 Types of Hashes**
- Common password hash formats:
  - **MD5** (`$1$`)
  - **SHA-256** (`$5$`)
  - **SHA-512** (`$6$`)
  - **bcrypt** (`$2a$`)
  - **NTLM** (`Windows hashes`)
  - **LM** (legacy Windows hashes)

### **2.2 Identifying Hashes**
- Using JtR to identify hash types:
  ```bash
  john --list=formats
  ```
- Using the `hashid` or `hash-identifier` tool:
  ```bash
  hashid <hash>
  ```

---

## **Module 3: Cracking Hashes with John the Ripper**
### **3.1 Basic Cracking**
- Cracking a single hash:
  ```bash
  john --format=<hash_format> --wordlist=<wordlist> <hashfile>
  ```
  Example:
  ```bash
  john --format=md5crypt --wordlist=/usr/share/wordlists/rockyou.txt hash.txt
  ```

- Cracking multiple hashes:
  ```bash
  john hashes.txt
  ```

### **3.2 Brute Force Attacks**
- Using brute force (incremental mode):
  ```bash
  john --incremental hash.txt
  ```
- Specifying character sets:
  ```bash
  john --incremental=digits hash.txt
  ```

### **3.3 Dictionary Attacks**
- Using a wordlist:
  ```bash
  john --wordlist=/path/to/wordlist.txt hash.txt
  ```
- Common wordlists:
  - **RockYou** (`/usr/share/wordlists/rockyou.txt`)
  - **SecLists** (GitHub repository with various wordlists)

### **3.4 Mask Attacks (Optimized Brute Force)**
- Using custom masks:
  ```bash
  john --mask=?l?l?l?l?l?l?l hash.txt
  ```
  - `?l` = lowercase letters
  - `?u` = uppercase letters
  - `?d` = digits
  - `?s` = special characters

### **3.5 Rules-Based Attacks**
- Using pre-defined rule sets:
  ```bash
  john --wordlist=rockyou.txt --rules hash.txt
  ```

---

## **Module 4: Cracking Windows Passwords**
### **4.1 Extracting Windows Hashes**
- Dumping NTLM hashes using `samdump2`:
  ```bash
  samdump2 SYSTEM SAM > hashes.txt
  ```
- Using `mimikatz` or `pwdump` tools

### **4.2 Cracking NTLM Hashes**
- Running JtR on NTLM hashes:
  ```bash
  john --format=nt hash.txt
  ```

---

## **Module 5: Advanced Techniques**
### **5.1 Cracking Online Passwords**
- Cracking SSH password hashes
- Capturing and cracking WPA2 Wi-Fi passwords
- Cracking ZIP file passwords:
  ```bash
  zip2john protected.zip > hash.txt
  john hash.txt
  ```

### **5.2 Using External Tools with John**
- Using `hashcat` alongside JtR
- Combining JtR with Hydra for online attacks

### **5.3 Customizing JtR**
- Writing custom rule files (`john.conf`)
- Optimizing performance with OpenMP
