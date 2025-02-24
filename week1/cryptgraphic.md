### **Week 5: Cryptography Basics - Explanation and Activities**  

Cryptography is an essential part of cybersecurity, used to secure communication, protect sensitive data, and ensure authenticity. Let's break down the core topics and activities for this week.

---

## **🔹 Topics:**

### **1️⃣ What is Cryptography?**
Cryptography is the science of securing information by transforming it into an unreadable format. It ensures **confidentiality, integrity, authentication,** and **non-repudiation** of data.

🔹 **Key Objectives of Cryptography:**
- **Confidentiality:** Keeps data private (e.g., encrypted messages).
- **Integrity:** Ensures data is not tampered with (e.g., hashing).
- **Authentication:** Confirms the sender's identity (e.g., digital signatures).
- **Non-repudiation:** Prevents denial of actions (e.g., signed emails).

---

### **2️⃣ Types of Cryptography: Symmetric vs. Asymmetric**
Cryptography is classified into **symmetric** and **asymmetric** encryption based on how keys are used.

#### **🔸 Symmetric Encryption (Secret-Key Encryption)**
- Uses **one single key** for encryption and decryption.
- Fast but requires securely sharing the key.
- Example: **AES (Advanced Encryption Standard).**

📌 **Example:**
🔐 Encrypt:  
`openssl enc -aes-256-cbc -salt -in file.txt -out file.enc -k secretkey`

🔓 Decrypt:  
`openssl enc -aes-256-cbc -d -in file.enc -out file_decrypted.txt -k secretkey`

#### **🔹 Asymmetric Encryption (Public-Key Encryption)**
- Uses **two keys**:  
  - **Public Key (for encryption).**  
  - **Private Key (for decryption).**
- Slower but more secure.
- Example: **RSA (Rivest-Shamir-Adleman).**

📌 **Example:**
🔐 Encrypt using a public key:  
`openssl rsautl -encrypt -pubin -inkey public.pem -in file.txt -out file.enc`

🔓 Decrypt using a private key:  
`openssl rsautl -decrypt -inkey private.pem -in file.enc -out file_decrypted.txt`

---

### **3️⃣ Hashing and Encryption**
🔹 **Encryption:** Converts plain text into ciphertext **(reversible process)**.  
🔹 **Hashing:** Converts input into a fixed-length hash value **(irreversible process)**.

#### **🔸 Hashing Algorithms (Used for Integrity)**
- **MD5 (Message Digest Algorithm 5)** → 128-bit (Weak, avoid using).
- **SHA-256 (Secure Hash Algorithm 256-bit)** → Stronger, widely used.

📌 **Example:**
Generate SHA-256 hash of a file:  
`sha256sum file.txt`

Verify a file’s integrity by comparing hashes.

---

### **4️⃣ Public Key Infrastructure (PKI)**
PKI is a system of digital certificates and encryption keys that secure communication over the internet.

🔹 **Key Components:**
- **Certificate Authority (CA):** Issues digital certificates.
- **Public/Private Key Pair:** Used in encryption.
- **Digital Signatures:** Used to verify authenticity.
- **SSL/TLS:** Used for HTTPS encryption.

🔹 **How it Works:**
1. A website requests a certificate from a CA.
2. The CA verifies the website's identity and issues a digital certificate.
3. The certificate contains the website's public key.
4. When users visit the website, their browser encrypts data using this public key.

---

## **🛠 Activities: Hands-on Cryptography Exercises**

### **1️⃣ Encrypt and Decrypt Files Using OpenSSL**
**Symmetric Encryption (AES-256):**
```bash
# Encrypt a file
openssl enc -aes-256-cbc -salt -in secret.txt -out secret.enc -k MyStrongPassword

# Decrypt the file
openssl enc -aes-256-cbc -d -in secret.enc -out secret_decrypted.txt -k MyStrongPassword
```

**Asymmetric Encryption (RSA):**
```bash
# Generate RSA key pair
openssl genpkey -algorithm RSA -out private.pem
openssl rsa -pubout -in private.pem -out public.pem

# Encrypt a file using the public key
openssl rsautl -encrypt -pubin -inkey public.pem -in message.txt -out message.enc

# Decrypt the file using the private key
openssl rsautl -decrypt -inkey private.pem -in message.enc -out message_decrypted.txt
```

---

### **2️⃣ Create and Verify Digital Signatures**
**Step 1: Create a Private and Public Key**
```bash
openssl genpkey -algorithm RSA -out private_key.pem
openssl rsa -pubout -in private_key.pem -out public_key.pem
```

**Step 2: Sign a File**
```bash
openssl dgst -sha256 -sign private_key.pem -out signature.bin document.txt
```

**Step 3: Verify the Signature**
```bash
openssl dgst -sha256 -verify public_key.pem -signature signature.bin document.txt
```

🔹 If the document is modified, the verification will fail, ensuring **integrity**.

---

## **Summary**
✔ Understand symmetric vs. asymmetric encryption.  
✔ Use OpenSSL for encryption and decryption.  
✔ Generate and verify hashes for integrity.  
✔ Work with digital signatures and PKI.  
