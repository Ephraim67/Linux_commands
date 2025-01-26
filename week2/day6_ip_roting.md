### **IP Routing in Linux**

IP routing is a critical process in Linux networking that determines how network packets travel from one host to another, ensuring data reaches its correct destination. The Linux kernel handles routing, using routing tables to decide the best path for packets across a network.

---

### **Understanding Routing Tables**

A routing table is essentially a set of rules that tells the system where to send packets based on their destination IP addresses. Each entry in the routing table specifies:
- **Destination**: The network or host to route packets to.
- **Gateway**: The next-hop address packets should take.
- **Interface**: The network interface to use for the route.

---

### **Essential Commands for IP Routing**

#### **1. View the Routing Table**
To display the current routing table, use:
```bash
ip route show
```
Example output:
```
default via 192.168.1.1 dev eth0
192.168.1.0/24 dev eth0 proto kernel scope link src 192.168.1.100
```
Explanation:
- **default via 192.168.1.1 dev eth0**: The default route (used for all packets without a specific route) goes through the gateway `192.168.1.1` on the interface `eth0`.
- **192.168.1.0/24 dev eth0**: Packets destined for the `192.168.1.0/24` network are sent directly through `eth0`.

#### **2. Add a New Route**
To add a static route to a specific network:
```bash
sudo ip route add 192.168.2.0/24 via 192.168.1.1 dev eth0
```
This command adds a route for the network `192.168.2.0/24`, specifying that packets should go through the gateway `192.168.1.1` on `eth0`.

#### **3. Delete a Route**
To delete a route:
```bash
sudo ip route del 192.168.2.0/24
```

#### **4. Set a Default Gateway**
To set or update the default route (default gateway):
```bash
sudo ip route add default via 192.168.1.1
```

#### **5. Flush All Routes**
To clear all routes from the routing table:
```bash
sudo ip route flush table main
```

---

### **Persistent Routing**

Changes made with the `ip` command are temporary and will be lost after a reboot. To make routes permanent, they must be added to network configuration files.  

For example, in Debian-based distributions, add routes to `/etc/network/interfaces`:
```conf
auto eth0
iface eth0 inet static
    address 192.168.1.100
    netmask 255.255.255.0
    gateway 192.168.1.1
    up ip route add 192.168.2.0/24 via 192.168.1.1 dev eth0
```

On Red Hat-based systems, use `/etc/sysconfig/network-scripts/ifcfg-eth0`:
```conf
DEVICE=eth0
BOOTPROTO=none
ONBOOT=yes
IPADDR=192.168.1.100
NETMASK=255.255.255.0
GATEWAY=192.168.1.1
```

---

### **Tips for IP Routing in Linux**
- Use `traceroute` or `ping` to debug and verify network routes.
- Use the `ip route` command for modern Linux systems, as it provides more functionality and is the preferred tool over `ifconfig` or `route`.
- Always test new routes before making them permanent to ensure they work as intended.
