## **Wireshark Display Filters Guide**

### **Introduction**
Wireshark is an advanced tool used to capture and analyze network traffic in real time. It provides a comprehensive view of what’s happening on the network, which is invaluable for cybersecurity professionals, network administrators, and analysts. One of Wireshark's most powerful features is its ability to apply **display filters**. Display filters allow users to isolate and examine specific types of network traffic, making the task of identifying potential security threats much easier.

In this lab, you will learn to capture network traffic, apply filters, and identify security issues using Wireshark.

---

### **Capturing Network Traffic**

Before diving into filtering and analysis, capturing network traffic is the first essential step. Wireshark captures packets from a specific network interface, and the data can be analyzed for patterns, security incidents, or anomalies.

1. **Start Wireshark**  
   First, ensure that you are in the **"Desktop" environment** to use Wireshark properly.

2. **Open Wireshark**  
   Open the terminal and run the following command:
   ```bash
   wireshark
   ```
   This will launch the Wireshark GUI. Make sure Wireshark is installed on your system.

3. **Select Network Interface**  
   After opening Wireshark, you will see a list of available network interfaces. These are the network adapters on your machine. The most common interfaces are **eth0**, **eth1**, **wlan0**, etc. Double-click on either `eth0` or `eth1` to start capturing traffic.

4. **Simulate Network Traffic**  
   To generate network traffic, open a second terminal window and use `curl` to visit a website. For example:
   ```bash
   curl www.google.com
   ```
   This will simulate HTTP traffic that you can capture and analyze in Wireshark.

5. **Observe Captured Traffic**  
   Once you start the capture, you should see packets appear in Wireshark. Each packet represents network traffic between devices on your network.

---

### **Applying Basic Display Filters**

Wireshark's display filters allow you to focus on specific traffic types, making it easier to analyze large amounts of data. Below are some basic display filters that will help you filter out the noise and zero in on specific network communications.

1. **Filter for HTTP Traffic**
   HTTP traffic is one of the most common protocols on the internet. If you want to filter out HTTP traffic, you can apply the following filter in the filter toolbar:
   ```bash
   http
   ```
   This filter will display only the packets related to HTTP communications. Once applied, you should see HTTP request and response packets.

2. **Filter Based on Source or Destination IP**
   If you want to focus on traffic coming from or going to a specific IP address, use the `ip.src` and `ip.dst` filters. For example, if you want to filter all traffic from a specific source IP (`192.168.1.100`) or destination IP (`192.168.1.100`), apply the following filter:
   ```bash
   ip.src == 192.168.1.100 || ip.dst == 192.168.1.100
   ```
   This will show all traffic from or to `192.168.1.100`.

3. **Filter for Specific Port Numbers**
   If you're interested in traffic on a specific port, for example, HTTP traffic (which typically uses port 80), you can filter traffic by specifying the port:
   ```bash
   tcp.port == 80
   ```
   This will show only the packets that are part of the HTTP communications, since HTTP typically uses port 80 for unencrypted web traffic. Similarly, you can filter for other ports by changing the port number.

---

### **Advanced Display Filters**

Wireshark's filtering system is very flexible and allows you to build complex filter expressions. You can combine multiple conditions to narrow down the traffic you are analyzing.

1. **Combining Filters for More Specific Traffic**
   You can combine multiple filters using logical operators such as **AND** (`&&`), **OR** (`||`), and **NOT** (`!`). For example, if you want to filter HTTP traffic from a specific source IP address (`192.168.1.100`) and destination port (`80`), use the following:
   ```bash
   http && ip.src == 192.168.1.100 && tcp.dstport == 80
   ```
   This filter shows only HTTP packets that are coming from `192.168.1.100` and are destined for port 80.

2. **Filter Based on Protocol Types**
   If you want to focus on a particular protocol (e.g., TCP, UDP, ICMP), you can filter by protocol name:
   ```bash
   tcp
   ```
   This will display only TCP traffic. Similarly, you can use:
   ```bash
   udp
   ```
   to show only UDP packets, or:
   ```bash
   icmp
   ```
   to filter for ICMP packets (used in ping requests).

3. **Filter by Packet Length**
   If you're looking for packets of a specific size, you can filter traffic by the length of the packet:
   ```bash
   frame.len == 128
   ```
   This will filter out packets with a length of exactly 128 bytes.

---

### **Identifying Security Threats**

Wireshark is a powerful tool not just for capturing traffic, but for detecting anomalies and security threats. By applying specific filters, you can search for unusual or potentially malicious traffic.

1. **Detecting Malicious Traffic**
   You can use filters to look for unusual traffic patterns such as:
   - Large volumes of ICMP packets (which may indicate a ping flood).
   - DNS queries to suspicious domains (potentially indicative of DNS tunneling).
   - TCP SYN packets that do not receive an ACK response (which could indicate a SYN flood attack).

2. **Analyzing Suspicious IPs**
   If you identify suspicious IP addresses in your network traffic, you can filter all traffic related to that IP. For example:
   ```bash
   ip.src == 192.168.1.50 || ip.dst == 192.168.1.50
   ```
   This will filter all traffic going to or coming from the suspicious IP `192.168.1.50`.

3. **Decrypting Encrypted Traffic (If Possible)**
   If you're dealing with SSL/TLS-encrypted traffic, Wireshark can decrypt it if you have access to the relevant keys. You can analyze the decrypted traffic in the same way as unencrypted traffic.

---

### **Conclusion**

Mastering display filters in Wireshark is essential for efficient network traffic analysis. By using the filters in this guide, you can quickly isolate and focus on specific types of traffic, which is especially helpful for identifying security incidents or network misconfigurations.

In this lab, you’ve learned how to:
1. Capture network traffic.
2. Apply basic display filters to isolate HTTP traffic, source/destination IPs, and ports.
3. Use advanced filters to narrow down traffic based on multiple criteria.
4. Detect potential security threats by analyzing the filtered traffic.

Wireshark is a powerful tool, and with the ability to filter traffic, you can significantly improve your ability to troubleshoot networks and identify security issues.
