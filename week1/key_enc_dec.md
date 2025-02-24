### **🔹 Encrypting & Decrypting Data Using RSA Key Pair**
Once you've generated your **private (`private_key.pem`)** and **public (`public_key.pem`)** keys, you can use them for **secure encryption and decryption**.

---

## **📌 Step 1: Create a Sample Message**
First, create a file with some text to encrypt.
```bash
echo "Hello, this is a secret message!" > message.txt
```

---

## **📌 Step 2: Encrypt the Message Using the Public Key**
We use the **public key** to encrypt the file.  
```bash
openssl rsautl -encrypt -pubin -inkey public_key.pem -in message.txt -out encrypted_message.bin
```
✅ **Now, `encrypted_message.bin` contains the encrypted message.**

🔴 **Why use the public key?**  
- Anyone can encrypt messages using the **public key**.  
- Only the owner of the **private key** can decrypt them.

---

## **📌 Step 3: Decrypt the Message Using the Private Key**
Now, decrypt the file using the **private key**.
```bash
openssl rsautl -decrypt -inkey private_key.pem -in encrypted_message.bin -out decrypted_message.txt
```
✅ **Check the decrypted message:**
```bash
cat decrypted_message.txt
```
Expected output:
```
Hello, this is a secret message!
```

---

## **📌 Step 4: Digitally Sign a File**
A **digital signature** proves that a message came from a trusted source and wasn’t altered.

### **Sign the message with the private key**
```bash
openssl dgst -sha256 -sign private_key.pem -out message.sig message.txt
```
✅ **Now, `message.sig` contains the digital signature.**

---

## **📌 Step 5: Verify the Signature Using the Public Key**
To verify the signature, the recipient uses your **public key**:
```bash
openssl dgst -sha256 -verify public_key.pem -signature message.sig message.txt
```
If valid, OpenSSL will return:
```
Verified OK
```

---

## **📌 Summary of Commands**
| **Action** | **Command** |
|------------|------------|
| Encrypt a message | `openssl rsautl -encrypt -pubin -inkey public_key.pem -in message.txt -out encrypted_message.bin` |
| Decrypt a message | `openssl rsautl -decrypt -inkey private_key.pem -in encrypted_message.bin -out decrypted_message.txt` |
| Sign a message | `openssl dgst -sha256 -sign private_key.pem -out message.sig message.txt` |
| Verify a signature | `openssl dgst -sha256 -verify public_key.pem -signature message.sig message.txt` |

---

### **📌 Real-World Applications**
- **🔐 Secure file sharing** (encrypt data before sending it).
- **📧 Email encryption (PGP-like security)**.
- **🛡️ Digital signatures for verifying software updates**.
