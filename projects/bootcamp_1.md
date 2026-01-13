## 1️⃣ LAN vs WAN vs DMZ

![Image](https://i.sstatic.net/aFNLH.jpg)

![Image](https://www.zenarmor.com/docs/assets/images/3-41a25cc48b21f3ab2f5b901dd6060bcb.png)

![Image](https://getlabsdone.com/wp-content/uploads/2021/12/image-371-edited.png.webp)

### 🔹 LAN (Local Area Network)

**What it is:**
The **internal, trusted network** inside an organization.

**Who/what is on the LAN:**

* Employee computers
* Printers
* Internal servers
* Admin systems
* Internal databases

**Key characteristics:**

* Not accessible from the internet
* Considered **trusted**
* Usually uses private IP addresses (e.g. 192.168.x.x)

**Real-world example:**
Your office Wi-Fi or wired office network.

👉 **Security idea:**
If an attacker reaches the LAN, they can do serious damage.
That’s why LAN must be protected the most.

---

### 🔹 WAN (Wide Area Network)

**What it is:**
An **external, untrusted network** — usually the **Internet**.

**Who/what is on the WAN:**

* External users
* Hackers
* Customers
* Unknown systems

**Key characteristics:**

* Completely untrusted
* Public IP addresses
* High risk

**Real-world example:**
The internet you use at home or on your phone.

👉 **Security idea:**
Never trust traffic coming from the WAN by default.

---

### 🔹 DMZ (Demilitarized Zone)

**What it is:**
A **buffer network** placed between the LAN and the WAN.

**Why it exists:**
To safely expose **public services** without exposing the LAN.

**Who/what is on the DMZ:**

* Web servers
* Mail servers
* Public APIs
* Test servers

**Key characteristics:**

* Semi-trusted
* Accessible from the WAN
* Restricted access to the LAN

**Real-world example:**
A company website that customers can access online.

---

### 🔐 Critical Security Principle (VERY IMPORTANT)

> **Even if the DMZ is compromised, attackers must NOT reach the LAN.**

That means:

* DMZ → LAN traffic is **blocked**
* LAN → DMZ traffic is **restricted**
* WAN → LAN traffic is **blocked**

This is called **network segmentation**.

---

## 2️⃣ Firewalls and Packet Filtering

![Image](https://media.geeksforgeeks.org/wp-content/uploads/1111-6.png)

![Image](https://marvel-b1-cdn.bc0a.com/f00000000310757/www.fortinet.com/content/dam/fortinet/images/cyberglossary/how-does-stateful-firewall-work_.png)

![Image](https://docs.netgate.com/pfsense/en/latest/_images/firewall-rule-separators.png)

### 🔹 What is a Firewall?

A **firewall** is a security device or software that:

* Sits between networks
* Decides what traffic is **allowed or blocked**
* Enforces security rules

In your bootcamp:
👉 **pfSense is the firewall**

---

### 🔹 What is Packet Filtering?

Packet filtering means the firewall **inspects each packet** and decides what to do with it based on rules.

The firewall looks at:

#### 📌 Source IP Address

* Where is the traffic coming from?
* Example: `192.168.1.10`

#### 📌 Destination IP Address

* Where is the traffic going?
* Example: `10.0.0.20`

#### 📌 Port Number

* What service is being used?
* Examples:

  * Port 80 → HTTP
  * Port 443 → HTTPS
  * Port 22 → SSH

#### Protocol

* How is the data being sent?
* Examples:

  * TCP (web, SSH)
  * UDP (DNS, streaming)
  * ICMP (ping)


### Example Firewall Decision

> “Allow traffic from LAN (192.168.1.0/24) to WAN on ports 80 and 443 using TCP.”

✔ Web browsing allowed
❌ Everything else blocked


## Stateful Firewall (pfSense)

### 🔹 What Does “Stateful” Mean?

A **stateful firewall**:

* Remembers active connections
* Tracks traffic states
* Automatically allows response traffic

Example:

1. A user on the LAN visits a website
2. Firewall allows the outgoing request
3. Firewall **remembers the connection**
4. The response from the website is allowed automatically

🔴 Without stateful tracking:

* You’d need rules for both directions
* Security becomes weak and complex

---

### 🔹 Why Stateful Firewalls Are Important

* Prevents spoofed responses
* Blocks unsolicited incoming traffic
* Improves security without many rules

👉 **pfSense is stateful by default**, which is why it’s widely used in enterprises.


* Why networks are segmented
* How attackers move between networks
* How firewalls enforce trust boundaries
* Why DMZ exists
* How pfSense protects the LAN
