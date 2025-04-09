## **Network Security Lab Exercise: Sniffers for Network and Protocol Analysis**

### **Background**
Network sniffers like **Wireshark** and **tcpdump** are tools used to capture and analyze network traffic. These tools help cybersecurity professionals inspect network protocols, identify suspicious activity, troubleshoot issues, and detect attacks like plaintext credentials or unencrypted traffic.

Understanding how to use sniffers is a core skill in both offensive (pentesting) and defensive (blue team) roles.

---

### **Learning Objectives**
By the end of this exercise, you will:
- Understand what sniffers are and what they capture
- Use a sniffer (Wireshark or tcpdump) to analyze traffic
- Identify different protocols (HTTP, DNS, TCP, etc.)
- Extract useful information from captured packets (e.g., credentials, hosts, visited sites)

---

### **Prerequisites**
- Wireshark installed on the host or VM
- Optionally: tcpdump installed on a Linux VM (Kali or Ubuntu)
- Internet connection or access to a test network environment
- A browser or client to generate basic HTTP, DNS, or FTP traffic

---

### **Tasks**

#### **Step 1: Start a Packet Capture**
Use **tcpdump** to capture traffic:
```bash
sudo tcpdump -i eth0 -nn -v -w capture.pcap
```

#### **Step 2: Generate Traffic**
Perform simple actions like:
- Visit a website (`localhost/dvwa`)
- Use `ping` or `nslookup` from terminal
- Use `ftp` or `telnet` to connect to a local server

This generates traffic Wireshark can observe.

---

#### **Step 3: Analyze Protocols**
In Wireshark:
- Use the **"Protocol"** column to filter for:
  - `http`
  - `dns`
  - `tcp`
  - `ftp` (if tested)
- Use the **Filter bar**, e.g.:
  ```plaintext
  http.request
  dns
  tcp.port == 21
  ```

Use:
1. Open **Wireshark**.
2. Load the captured file.
3. Try to:
- Identify the source and destination IPs
- Observe TCP handshakes
- See what sites or hosts were contacted

#### **Step 4: Extract Information**
From the captured traffic:
- Find the **Host** and **User-Agent** headers from HTTP requests
- Look for **GET** or **POST** requests and any visible parameters
- Try to spot credentials if basic auth

---

### **Validation Checklist**

| Task                                    | Expected Output or Evidence                        |
|-----------------------------------------|----------------------------------------------------|
| Sniffer launched and packets captured   | Screenshot or `.pcap` file showing packet data     |
| Protocols filtered and identified       | List of protocols observed (HTTP, DNS, TCP, etc.)  |
| Packet details analyzed                 | IP addresses, flags, ports noted       |
| Data extracted from a packet            | Hostname, URL path, or login credentials shown     |
