### **Setting Up a Metasploitable Network and Capturing Traffic with Wireshark in Kali**  
---

## **🛠 Requirements**  
✅ **Virtual Machines (VMs)**  
- **Kali Linux** (Attacker Machine)  
- **Metasploitable 2** (Target Machine)  

✅ **Tools Needed**  
- **VMware/VirtualBox** (To run the virtual machines)  
- **Wireshark** (For packet capture)  
- **Metasploit Framework** (For attacking the target)  

✅ **Network Configuration**  
- Set both VMs to **Host-Only Network** or **NAT with Port Forwarding**  
- Kali will act as the **attacker**  
- Metasploitable will be the **victim**  

---

## **1️⃣ Setting Up Metasploitable 2**  
📌 *Metasploitable is a deliberately vulnerable Linux machine for penetration testing.*  

### **Steps to Install Metasploitable:**  
1️⃣ **Download Metasploitable 2:**  
   - Get the **OVA file** from [here](https://sourceforge.net/projects/metasploitable/).  

2️⃣ **Import into VirtualBox/VMware:**  
   - Open VirtualBox → `File` → `Import Appliance` → Select **Metasploitable.ova**  

3️⃣ **Network Settings:**  
   - Go to **Settings → Network**  
   - Set **Adapter 1** to **Host-Only Adapter** (or NAT)  

4️⃣ **Start Metasploitable and Get IP Address:**  
   - Login with:  
     ```plaintext
     Username: msfadmin  
     Password: msfadmin
     ```  
   - Run:  
     ```bash
     ifconfig
     ```
   - Note the **IP address** (e.g., `192.168.56.101`).  

---

## **2️⃣ Setting Up Kali Linux**  
📌 *Kali Linux will be used to attack Metasploitable and capture network traffic.*  

### **Steps:**  
1️⃣ **Open Kali Linux in VirtualBox/VMware**  
2️⃣ **Network Settings:**  
   - Set **Adapter 1** to **Host-Only Adapter** (same as Metasploitable)  
3️⃣ **Confirm Network Connection:**  
   - Open a terminal in Kali and ping Metasploitable:  
     ```bash
     ping 192.168.56.101
     ```
   - If successful, your network is properly configured.  

---

## **3️⃣ Capturing Network Traffic with Wireshark**  
📌 *Wireshark will capture all traffic between Kali and Metasploitable.*  

### **Steps to Capture Traffic:**  
1️⃣ **Open Wireshark in Kali:**  
   ```bash
   wireshark &
   ```  
2️⃣ **Select Your Network Interface:**  
   - Choose `eth0` (or the interface that connects to Metasploitable).  
3️⃣ **Start Capturing:**  
   - Click **Start Capture** (Blue Shark Fin icon).  

---

## **4️⃣ Simulating an Attack & Capturing Credentials**  
📌 *Now, we will simulate a login attack and capture credentials.*  

### **Step 1: Connect to Metasploitable via Telnet (Unencrypted Protocol)**  
1️⃣ In Kali, run:  
   ```bash
   telnet 192.168.56.101
   ```  
2️⃣ Enter **any username & password** (doesn’t matter if correct).  

### **Step 2: Capture Credentials in Wireshark**  
1️⃣ In Wireshark, apply a filter to see Telnet traffic:  
   ```plaintext
   telnet
   ```  
2️⃣ Look for **login credentials** in plain text!  

---

## **5️⃣ Advanced Attack: Exploiting SSH with Metasploit**  
📌 *Now, we will use Metasploit to attack an SSH service and capture network traffic.*  

### **Step 1: Scan for Open Ports on Metasploitable**  
1️⃣ In Kali, run:  
   ```bash
   nmap -sV 192.168.56.101
   ```  
2️⃣ Look for **port 22 (SSH)** in the results.  

### **Step 2: Run an SSH Brute Force Attack**  
1️⃣ Open Metasploit:  
   ```bash
   msfconsole
   ```  
2️⃣ Use the SSH brute-force module:  
   ```bash
   use auxiliary/scanner/ssh/ssh_login
   set RHOSTS 192.168.56.101
   set USERNAME msfadmin
   set PASSWORD msfadmin
   run
   ```  

### **Step 3: Capture SSH Attack in Wireshark**  
1️⃣ In Wireshark, apply the filter:  
   ```plaintext
   ssh
   ```  
2️⃣ Observe the brute-force attack attempts!  

---

## **🎯 Outcome**  
✔️ Successfully captured **Telnet & SSH traffic**.  
✔️ Extracted **plaintext credentials** from unencrypted traffic.  
✔️ Identified attack patterns using **Wireshark**.  
