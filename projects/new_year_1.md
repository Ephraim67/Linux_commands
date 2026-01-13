## Project Scope

### In Scope

* Network services on Metasploitable
* Known vulnerable applications and services

### Out of Scope

* Real production systems
* Internet-facing targets
* Social engineering



## Penetration Testing Phases


## Information Gathering (Reconnaissance)

### Objective

Identify:

* IP address of the target
* Open ports
* Running services
* Service versions

### Tools Used

* Network scanners
* Service enumeration tools
* OS fingerprinting utilities

### Findings (Example)

| Port | Service | Version | Notes                      |
| ---- | ------- | ------- | -------------------------- |
| 21   | FTP     | vsftpd  | Anonymous login enabled    |
| 22   | SSH     | OpenSSH | Weak credentials suspected |
| 80   | HTTP    | Apache  | Test pages exposed         |

**Key Learning:**
Open services increase attack surface.

## Scanning & Enumeration

### Objective

Gather **deeper details** about exposed services.

### Activities

* Enumerate:

  * FTP users
  * Web directories
  * Database services
* Identify misconfigurations

### Tools Used

* Service-specific enumeration tools
* Web vulnerability scanners

### Example Findings

* Anonymous FTP access enabled
* Default web pages exposed
* Old software versions detected


## Vulnerability Identification

### Objective

Map discovered services to **known vulnerabilities**.

### Methods

* Compare service versions against:

  * CVE databases
  * Security advisories
* Automated vulnerability scanning

### Sample Vulnerabilities Identified

| Service  | Vulnerability          | Severity |
| -------- | ---------------------- | -------- |
| FTP      | Backdoor vulnerability | Critical |
| Web App  | Command execution flaw | High     |
| Database | Default credentials    | High     |

**Key Learning:**
Outdated software is a major risk.



## Exploitation

### Objective

Demonstrate that a vulnerability is **exploitable**.

### Controlled Actions

* Use exploitation frameworks **only against Metasploitable**
* Obtain limited shell access
* Validate impact (no data destruction)

### Results

* Remote access achieved
* Privilege escalation possible due to misconfiguration



## Post-Exploitation Analysis

### Objective

Understand the **impact**, not to cause damage.

### Observations

* System allowed unauthorized access
* Poor privilege separation
* Weak default configurations



## Risk Analysis

### Risk Summary Table

| Risk                | Impact | Likelihood |
| ------------------- | ------ | ---------- |
| Unauthorized access | High   | High       |
| Data compromise     | High   | Medium     |
| Service disruption  | Medium | Medium     |



## Mitigation & Recommendations

### Security Improvements

* Disable unused services
* Enforce strong authentication
* Regular patching and updates
* Firewall and intrusion detection
* Principle of least privilege



### Final Deliverables (What You Submit)

1. **Penetration Testing Report (PDF / DOC)**

   * Executive Summary
   * Methodology
   * Findings
   * Risk ratings
   * Recommendations

2. **Network Diagram**

3. **Screenshots (non-sensitive)**

4. **Reflection Section**

   * What you learned
   * Defensive takeaways

