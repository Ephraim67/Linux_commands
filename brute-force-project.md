# Webdeves Ethical Hacking Final Project (Team Light)

## **Project Title: Brute-Force Attack Simulator with Defense Mechanism**

### **Objective**:  
You wiil learn how brute-force attacks work by simulating password attacks on a login system **you build**, and then showing how to **defend** against them with rate-limiting, captchas, and account locking.

### **Key Components**:

#### Attacker Side (Simulated with Python):
1. **Login Bruteforcer**:
   - Attempts username/password logins from a wordlist.
   - Logs success/failure and time taken.
   - Can target a Flask login form or SSH via `paramiko`.

2. **Dictionary Attack**:
   - Uses `rockyou.txt` or a custom wordlist.
   - Tries combinations and measures how many tries are needed.

#### Defender Side:
3. **Secure Login Server (Flask)**:
   - Basic web login page.
   - Detects brute-force attempts.
   - Implements:
     - Rate limiting (e.g., 5 attempts per IP)
     - Account lockout after N failed attempts
     - Optional CAPTCHA or 2FA simulation

4. **Logging & Analysis**:
   - Log all login attempts with timestamps and IPs.
   - Graph login success/failure over time.
   - Show how attackers are blocked after thresholds.


### **Tools & Libraries**:
- `Flask` – web server
- `paramiko` – for SSH brute-force (optional)
- `requests` – for HTTP POST attacks
- `sqlite3` – to store user credentials and lockout state
- `argparse` – for attack simulation CLI
- `matplotlib` or `seaborn` – to graph attacks vs defenses


### **Setup Instructions**:
- Build a local Flask login app.
- Write a bruteforcer that targets the form.
- Then implement security features and rerun the attack to demonstrate prevention.
- Bonus: Deploy it on a private LAN or local VM lab.

### **Deliverables**:
- Python code for:
  - Flask login system
  - Brute-force attack script
  - Defense mechanisms
- Screenshots of attack logs and blocked attempts
- Analysis report comparing success rates before vs after defense

### **Real-World Impact**:
- Shows how **automated attacks work**
- Reinforces need for security features like rate limiting and account lockout
- Builds awareness of **OWASP Top 10** (Brute Force, Credential Stuffing)
- You get to play both attacker and defender — **Red Team vs Blue Team**
