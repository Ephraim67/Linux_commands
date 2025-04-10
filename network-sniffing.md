# Webdeves Ethical Hacking Final Project (Team Red)

## **Project Title: Packet Sniffer & Analyzer**

### **Objective**:  
Build a Python-based **packet sniffer** that captures, analyzes, and logs network traffic to help identify suspicious activity and learn how attackers spy on or manipulate data in transit.

### **Core Features**:

1. **Packet Capture**:
   - Use `scapy` or `pyshark` to sniff packets in real time.
   - Capture Ethernet, ARP, IP, TCP, UDP, and HTTP traffic.

2. **Protocol Analyzer**:
   - Parse and print packet details (source, destination, protocol, payload).
   - Detect unencrypted credentials (like HTTP Basic Auth).
   - Highlight common reconnaissance activity (e.g., ping sweeps, port scans).

3. **Suspicious Activity Detection**:
   - Flag:
     - DNS Tunneling
     - ARP Spoofing
     - Port scanning attempts
     - MITM indicators

4. **Logging & Exporting**:
   - Save captured traffic to `.pcap` or `.csv`.
   - Allow filtering by protocol or IP.
   - Timestamped logs for forensic replay.

5. **Bonus Features (Advanced)**:
   - Live dashboard with graphs (Flask + Chart.js).
   - PCAP file upload and offline analysis.
   - Alert system for detected anomalies.

### **Tools & Libraries**:
- `scapy` – low-level packet crafting/sniffing
- `pyshark` – wrapper around tshark for deep packet parsing
- `socket` – IP-level network interactions
- `pandas` – traffic log analysis
- `matplotlib` or `Plotly` – for visualizations


### **Setup Instructions**:
- Run in a **controlled lab network** (e.g., Kali + Metasploitable or DVWA).
- Capture only local subnet traffic.
- Teach how to filter with `Wireshark` and compare with the tool’s output.


### **Deliverables**:
- Fully documented Python source
- Interface or CLI for capturing and analyzing
- Exportable PCAP/CVS logs
- Sample attack detection outputs
- Report explaining packet structure and threats

### **Real-World Impact**:
- Deep understanding of **network layers and protocols**
- Foundation for IDS/IPS systems
- Insight into how **hackers and defenders use packet sniffing**
- Creates a springboard for projects like:
  - Intrusion detection systems
  - MITM simulators
  - Network forensics tools
