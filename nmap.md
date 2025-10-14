# 1 — Service & Version Enumeration (Advanced Nmap + NSE)

## Learning objectives

* Understand service/version discovery and why it matters.
* Use Nmap flags `-sV`, `-sC`, `--script`, and timing options to enumerate services and probe versions.
* Use Nmap Scripting Engine (NSE) for targeted checks (banners, vulns, auth issues).
* Interpret Nmap output and turn it into prioritized findings.

## Tools

* `nmap` (with NSE)
* `grep`, `awk` (for parsing)
* A lab VM (e.g., Metasploitable2/3 or a small web/ssh test VM)
* (Optional) `masscan` for large-scale discovery before Nmap

## Lesson flow

1. Quick review: TCP handshake and why banner/version info helps (10 min).
2. Demo Nmap commands and explain flags (20 min).
3. Students run guided lab (40–60 min).
4. Discuss results, false positives, and next steps (20 min).

## Core Nmap commands to teach

* `nmap -Pn -sV -p 1-65535 TARGET` — full-range service/version scan (explain time).
* `nmap -sS -sV -O -A TARGET` — stealth SYN, version detection, OS detect, aggressive script set.
* `nmap -sV --version-all TARGET` — exhaustive version probes.
* `nmap --script=banner,safe TARGET -p 80,443,22` — run specific NSE scripts (banner, safe scripts).
* `nmap --script vuln TARGET` — (lab only!) run vulnerability-themed scripts — emphasize permitted use.

## Lab exercise

1. Lab setup: student has VM `10.10.10.5` with SSH and HTTP.
2. Command: `nmap -Pn -sV -p22,80 10.10.10.5 -oN nmap_basic.txt`
3. Inspect `nmap_basic.txt`. Look for `SERVICE`, `VERSION`, and `OS` lines.
4. Run NSE examples:

   * `nmap -sV --script=banner 10.10.10.5 -p22,80 -oN nse_banner.txt`
   * `nmap -sV --script=http-enum --script-args http-enum.maxdepth=2 10.10.10.5 -p80 -oN http_enum.txt`

## Expected outputs 

```
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 7.2p2 Ubuntu (protocol 2.0)
80/tcp open  http    Apache httpd 2.4.18 ((Ubuntu))
...
| http-enum: /admin (Potentially interesting)
|_http-enum: /test/
```

## Case study — “Retail PoS Lab” (example)

**Scenario:** A simulated retail Point-of-Sale VM (10.10.20.10) is part of the lab network. Instructor wants students to discover exposed management interfaces and versions.

**Actions taken**

1. `nmap -Pn -sS -sV -O 10.10.20.10 -oN retail_nmap.txt`
2. `nmap -sV --script=http-enum,http-server-header 10.10.20.10 -p 80,8080 -oN retail_http.txt`

**Findings**

* Port 8080 open: `Apache Tomcat 7.0.23` (old) — likely vulnerable to known issues.
* `/manager/` discovered by `http-enum` and returns a login page.
* SSH version `OpenSSH 6.6` — older but not necessarily exploitable.

**Risk & priority**

* Tomcat manager UI exposed publicly → **High** (privileged web admin could be brute-forced or exploited).
* Action: Block public access to 8080, restrict to management subnet, upgrade Tomcat, enforce strong passwords and 2FA.

**Sample remediation steps**

* Firewall rule: deny inbound 8080 from internet, allow only management VLAN.
* Patch Tomcat to current supported version.
* Enforce least privilege and rotate credentials.



# 2 — Banner Grabbing & Basic Manual Enumeration

## Learning objectives

* Learn manual techniques to collect service banners and meta-data (`nc`, `telnet`, `curl`, `openssl s_client`).
* Understand how to interpret service banners and cross-check with vendor CVE lists.
* Practice using small scripts and manual parsing to augment automated tools.

## Tools

* `netcat` (`nc`)
* `telnet`
* `curl`
* `openssl s_client`
* `ncat` (from nmap) or `socat` for TLS tests
* Text editors, `grep`

## Lesson flow (60–90 minutes)

1. Theory: what banners reveal, why they can be misleading (10 min).
2. Demo of `nc`, `curl`, `openssl s_client` (15 min).
3. Hands-on exercises (30–45 min).
4. Interpreting results and next steps (15 min).

## Practical commands & examples

* TCP banner grab: `echo -e "\r\n" | nc -v TARGET 25` (check SMTP banner)
* HTTP header / banner: `curl -I http://TARGET/` or `curl -s -D - http://TARGET/ -o /dev/null`
* TLS certificate check: `openssl s_client -connect TARGET:443 -servername example.com </dev/null 2>/dev/null | openssl x509 -noout -text`
* FTP: `nc TARGET 21` then wait or send `QUIT`.

## Lab exercise

1. Target webserver `10.11.11.11:80`.
2. `curl -I http://10.11.11.11/` → collect Server header.
3. `echo -e "\r\n" | nc 10.11.11.11 25` → observe SMTP banner.
4. `openssl s_client -connect 10.11.11.11:443 -servername 10.11.11.11 </dev/null | openssl x509 -noout -subject -dates -issuer`

## Expected results (examples)

* `Server: Apache/2.4.18 (Ubuntu)`
* SMTP: `220 mail.example.com ESMTP Postfix (Ubuntu)`

## Case study — “Hidden Admin via Header” (example)

**Scenario:** A small web app returns unusual headers. Student is tasked to manually enumerate and interpret.

**Actions**

1. `curl -I http://192.168.56.101/`

   ```
   HTTP/1.1 200 OK
   Date: ...
   Server: Apache/2.4.18 (Ubuntu)
   X-Powered-By: PHP/5.6.30
   X-Backend-Server: admin01.local
   ```
2. `openssl s_client -connect 192.168.56.101:443 -servername example.local </dev/null | openssl x509 -noout -subject -issuer -dates`

**Findings**

* `X-Backend-Server: admin01.local` leaks internal hostnames (information disclosure).
* PHP 5.6 used — end-of-life → security risk.
* TLS cert subject shows CN `admin01.local` — indicates internal certificate reused publicly.

**Risk & remediation**

* Information disclosure via headers: moderate risk; remove internal headers or proxy them.
* PHP EoL: plan upgrade and patch.
* Replace certificates, scope cert validity to internal networks only; ensure public endpoints don’t reveal internal hostnames.

---

# 3 — Vulnerability Scanning & Assessment (OpenVAS/ GVM / Nessus overview)

## Learning objectives

* Understand automated vulnerability scanners: purpose, strengths, and limitations.
* Learn how to run scans in a lab safely and how to triage scanner findings (false positives, exploitability, business impact).
* Learn to produce a concise vulnerability report with remediation priorities.

## Tools

* OpenVAS / Greenbone Vulnerability Management (GVM) — open source
* Nessus (commercial/edu) — overview & output interpretation
* Nmap output (to feed into scanners)
* CSV/HTML for reports

## Lesson flow

1. Overview of scanners, credentialed vs uncredentialed scans (20 min).
2. Demo: launching an OpenVAS scan on a lab host (30–40 min).
3. Lab: students run scans, examine top findings, map CVSS, determine remediation (60–80 min).
4. Discussion: false positives, safe scans, rate-limiting (20 min).

## Lab exercise (OpenVAS/GVM)

1. Instructor provides lab scanner UI URL (e.g., `https://labscanner:9392`) and credentials.
2. Students import target IP or upload `nmap` XML for targeted scanning.
3. Run an uncredentialed scan and export results to CSV/XML.
4. Optional: perform a credentialed scan with read-only SSH credentials (demonstrate differences).

## How to triage & prioritize

* Use CVSS as a guide but consider exploitability, exposure (internet-facing?), asset value.
* Categories: Critical (patch/mitigate immediately), High (schedule patch), Medium (monitor/plan), Low (document).
* Verify high findings manually before action (avoid false positives causing downtime).

## Example scanner output (simplified)

```
- CVE-2017-5638 (Apache Struts RCE) — CVSS 10.0 — Confirmed vulnerable on port 8080 (Tomcat).
- TLS: Weak protocol SSLv3 enabled — CVSS 5.3 — Remote Mitigation: disable SSLv3
- Outdated OpenSSH (CVE-YYYY-XXXX) — CVSS 7.5 — further investigation required
```

## Case study — “Corporate Web App Scan” (example)

**Scenario:** Internal pentest team runs OpenVAS against `webapp.corp.lab` (10.10.50.20) after service inventory. Scan is uncredentialed, then credentialed.

**Actions**

1. Run Nmap to identify services: `nmap -sV 10.10.50.20 -oX services.xml`
2. Import `services.xml` into OpenVAS and run an uncredentialed scan.
3. Run a credentialed scan using a read-only app user to surface missing patches and configuration issues.

**Findings**

* OpenVAS uncredentialed: reports possible Struts vulnerability on port 8080 — needs validation.
* Credentialed scan: shows installed package `struts-core 2.x` with known vulnerable version and able to confirm exploitability in lab environment.
* Several missing Windows patches (MS patch IDs) discovered via credentialed scan.
* TLS configuration allowed TLS 1.0 and weak ciphers.

**Risk & remediation**

* Struts RCE → **Critical**: apply vendor patch or isolate service, apply WAF rule to block exploit patterns.
* Patch management: prioritize Windows updates per asset value; schedule emergency patch window.
* TLS: disable TLS1.0/1.1, enable TLS1.2+ and strong cipher suites, add HSTS.

**Reporting snippet (example)**

```
Finding: Apache Struts RCE (CVE-2017-5638)
Severity: Critical — CVSS: 10.0
Evidence: Version struts-core 2.x identified, payload test confirms remote command response.
Impact: Remote code execution leading to full system compromise.
Remediation: Immediately upgrade Struts to patched version or apply vendor-provided mitigation. Restrict access to management ports (8080) via firewall; implement Web Application Firewall rules to block exploit signatures.
```

---

# Ethics & Safety (always say this)

* Only scan/test systems you own or have explicit written permission to test.
* Use isolated lab networks (VMs, VLANs) when teaching scanning and enumeration.
* Avoid destructive tests on production systems — vulnerability scanners can cause instability.

---

# Assessment rubrics (short)

1. **Nmap/NSE practical**

   * Discovery completeness (30%)
   * Correct use of flags & scripts (20%)
   * Interpretation of results (30%)
   * Clear remediation recommendations (20%)

2. **Banner grabbing exercise**

   * Correct banners captured (30%)
   * Accurate identification of risks (30%)
   * Cross-check to known advisories (20%)
   * Written summary (20%)

3. **Vulnerability scan & triage**

   * Proper scan configuration (credentialed/uncredentialed) (25%)
   * Top 5 prioritized findings with justification (35%)
   * Remediation plan with timelines (25%)
   * Evidence and false positive handling (15%)
