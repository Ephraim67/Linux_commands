### **Understanding Subnetting in Linux**

**Subnetting** is a fundamental technique in networking that divides a larger network into smaller, manageable subnetworks (subnets). It helps in:  
- **Improving Performance**: Reduces network congestion by isolating traffic.  
- **Enhancing Security**: Restricts access between subnets.  
- **Efficient IP Address Management**: Prevents wastage of IP addresses in large networks.

Linux systems often play a central role in managing subnets within an Internet Protocol (IP) addressing scheme, making subnetting essential in complex network setups.

---

### **Benefits of Subnetting**
1. **Efficient Use of IP Addresses**: Allows multiple subnets within a single IP address range.  
2. **Reduced Broadcast Domains**: Isolates traffic to specific subnets, reducing unnecessary traffic.  
3. **Enhanced Security**: Limits communication between subnets, improving data privacy.

---

### **Subnetting in Practice**

1. **IP Address and Subnet Mask**  
   - An IP address (e.g., `192.168.1.1`) identifies a device.  
   - A subnet mask (e.g., `255.255.255.0`) defines the size of the network and subnet.  

   Example:  
   - **IP Address**: `192.168.1.1`  
   - **Subnet Mask**: `255.255.255.0`  
     - Total addresses in the subnet: 256 (from `192.168.1.0` to `192.168.1.255`).  
     - Usable addresses: 254 (excluding `.0` and `.255` for network and broadcast).

2. **CIDR Notation**  
   - Subnet masks are often written in CIDR (Classless Inter-Domain Routing) notation.  
   - Example: `/24` = `255.255.255.0` (24 bits for the network, 8 bits for hosts).

---

### **Linux Commands for Subnetting**

#### **1. Display the Current Routing Table**  
The `route` command shows routing rules and subnet information:  
```bash
route -n
```
- **Example Output**:
  ```
  Kernel IP routing table
  Destination     Gateway         Genmask         Flags Metric Ref    Use Iface
  0.0.0.0         192.168.1.1     0.0.0.0         UG    100    0        0 eth0
  192.168.1.0     0.0.0.0         255.255.255.0   U     0      0        0 eth0
  ```

#### **2. Add a New Subnet**  
Use the `route` command to add a route for a subnet:  
```bash
sudo route add -net 192.168.2.0/24 gw 192.168.1.1
```
- **Explanation**:  
  - `192.168.2.0/24`: Subnet address and mask.  
  - `192.168.1.1`: Gateway for the subnet.  

#### **3. Verify Network Interfaces and Subnet Configuration**  
```bash
ip addr
```
- Shows IP address and subnet mask for all interfaces.

---

### **Practical Example: Subnetting in Linux**
Suppose you want to divide the network `192.168.1.0/24` into two subnets:  
- Subnet 1: `192.168.1.0/25` (128 addresses)  
- Subnet 2: `192.168.1.128/25` (128 addresses)  

1. Configure Subnet 1:  
   ```bash
   sudo route add -net 192.168.1.0/25 gw 192.168.1.1
   ```

2. Configure Subnet 2:  
   ```bash
   sudo route add -net 192.168.1.128/25 gw 192.168.1.1
   ```

3. Verify:  
   ```bash
   route -n
   ```

---

### **Final Notes**  
- **Automation**: Use configuration files like `/etc/network/interfaces` (Debian/Ubuntu) or `/etc/sysconfig/network-scripts/` (RHEL/CentOS) for permanent routing rules.  
- **Replacement for `route`**: The `ip route` command is a modern alternative:  
  ```bash
  ip route add 192.168.2.0/24 via 192.168.1.1
  ```  
