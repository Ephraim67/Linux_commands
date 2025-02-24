### **Beginner Cryptography Project: Secure File Transfer with Encryption & Digital Signatures**  

This project will help students apply cryptography concepts practically by encrypting, signing, and verifying files before securely transferring them.

---

## **📌 Project: Secure File Transfer System**
### **🛠 Goal:**
- Encrypt a file before sending it.
- Sign the file using a digital signature.
- Verify the signature and decrypt the file upon receipt.

---

## **1️⃣ Project Breakdown**
### **📌 Step 1: Generate RSA Keys**
Each user needs a **public-private key pair** for encryption and signing.

```bash
# Generate private key (receiver)
openssl genpkey -algorithm RSA -out receiver_private.pem

# Extract public key (for sender to use)
openssl rsa -pubout -in receiver_private.pem -out receiver_public.pem

# Generate private key (sender)
openssl genpkey -algorithm RSA -out sender_private.pem

# Extract public key (for receiver to use)
openssl rsa -pubout -in sender_private.pem -out sender_public.pem
```

---

### **📌 Step 2: Encrypt the File (Sender)**
Before sending a file, encrypt it using the **receiver's public key** so only the receiver can decrypt it.

```bash
# Encrypt the file with the receiver's public key
openssl rsautl -encrypt -pubin -inkey receiver_public.pem -in secret.txt -out secret.enc
```

---

### **📌 Step 3: Create a Digital Signature (Sender)**
To ensure the file is authentic and not tampered with, the sender signs the encrypted file using their **private key**.

```bash
# Create a signature for the encrypted file
openssl dgst -sha256 -sign sender_private.pem -out secret.sig secret.enc
```

---

### **📌 Step 4: Send Files Securely**
The sender now sends **two files** to the receiver:
1. `secret.enc` (Encrypted file)
2. `secret.sig` (Digital signature)

---

### **📌 Step 5: Verify the Signature (Receiver)**
Before decrypting, the receiver **verifies** the sender's signature using the sender’s **public key**.

```bash
# Verify the signature
openssl dgst -sha256 -verify sender_public.pem -signature secret.sig secret.enc
```
✅ If the signature is valid, the file is authentic and not modified.

---

### **📌 Step 6: Decrypt the File (Receiver)**
After verification, the receiver decrypts the file using their **private key**.

```bash
# Decrypt the file with the receiver’s private key
openssl rsautl -decrypt -inkey receiver_private.pem -in secret.enc -out secret_decrypted.txt
```
✅ The original file is now recovered.

---

## **2️⃣ Project Enhancements**
To take it further:
- Build a **Python script** to automate encryption, signing, and verification.
- Implement **AES symmetric encryption** for large files instead of RSA.
- Use a **web interface** to encrypt, sign, and verify files.

---

## **3️⃣ Learning Outcomes**
✔ Understand **RSA encryption** and **digital signatures**.  
✔ Use **OpenSSL for secure file transfers**.  
✔ Verify data authenticity with **signatures**.  
✔ Gain hands-on cryptography experience.
