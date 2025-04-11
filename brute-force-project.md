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

## **PHASE 1: Build the Target – Flask Login Server**

### Goal: Create a vulnerable login system for the bruteforce to attack.

**Steps:**
1. Set up a simple Flask app:
   - One `/login` route with a login form (username/password).
   - Basic authentication against hardcoded or SQLite-stored credentials.
2. Store:
   - `users.db` with usernames and hashed passwords (`werkzeug.security`).
   - A `login_attempts` table with IP address and timestamp.

**Example snippet:**
```python
@app.route('/login', methods=['POST'])
def login():
    username = request.form['username']
    password = request.form['password']
    # validate with DB and return success/fail
```

## **PHASE 2: Simulate the Attack – Brute-Forcer Script**

### Goal: Create an attacker script to bruteforce the login page.

**Steps:**
1. Use `requests` to send POST data to the `/login` endpoint.
2. Load wordlist (`rockyou.txt` or custom list).
3. Log results, time taken, and success/failure.

**Python script example (CLI):**
```python
python bruteforce.py --url http://localhost:5000/login --username admin --wordlist rockyou.txt
```

Use `argparse` to configure the attack. Optionally, support attacking SSH using `paramiko`.

## **PHASE 3: Add Logging and Metrics**

### Goal: Track and visualize attack attempts.

1. Log every login attempt to a file and/or database:
   - IP, username, success/fail, timestamp.
2. Use `matplotlib` or `seaborn` to plot:
   - Total attempts over time.
   - Success/failure rate before and after defense mechanisms.

## **PHASE 4: Implement Defense Mechanisms (Blue Team Mode)**

### Goal: Make the login system more secure.

**Defense Features to Implement:**

| Feature             | Description                                                       |
|---------------------|-------------------------------------------------------------------|
| Rate Limiting       | Limit to 5 attempts per IP per 60 seconds.                        |
| Account Lockout     | Lock account for X minutes after Y failed attempts.               |
| CAPTCHA (Bonus)     | Show CAPTCHA after 3 failed tries (can be simulated).             |
| 2FA (Bonus)         | Add a mock OTP after password success.                            |

Use `Flask-Limiter` for rate-limiting or write your own middleware logic.

## **PHASE 5: Re-run Attacks & Compare Results**

### Goal: Show the impact of your defenses.

1. Run the same brute-force script against the hardened Flask app.
2. Capture:
   - Time taken
   - Number of attempts allowed
   - Whether the user was blocked or not
3. Generate graphs to compare before vs after.

## **EXTRA (Optional)**

- Build a **dashboard** (Flask + Charts.js or matplotlib images) to show real-time attack logs.
- Deploy the setup on a LAN with VMs (attacker Kali, target Ubuntu/Flask).
- Integrate CVE demos or OWASP Top 10 reference in your README.

## Project Folder Structure

```
brute-force-simulator/
├── flask_server/
│   ├── app.py
│   ├── database.py
│   ├── login.html
│   ├── defense.py
│   └── utils.py
├── attacker/
│   ├── bruteforce.py
│   ├── wordlists/
│   └── logs/
├── analysis/
│   ├── visualize.py
│   └── attack_stats.csv
├── README.md
├── requirements.txt
└── screenshots/
```

## Bonus Tips

- Add colored terminal output (e.g., `colorama`) in the bruteforce script for UX.
- Rate limit with `Flask-Limiter`:  
  ```python
  limiter = Limiter(app, key_func=get_remote_address, default_limits=["5 per minute"])
  ```
- Use `sqlite3` to persist user/attempt data with timestamps.
- Track IPs using `request.remote_addr`.

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
