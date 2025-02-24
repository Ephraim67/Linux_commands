### **🔹 Creating a Normal RSA Key Pair (Private & Public Key)**
If you just need a **basic key pair** (not a self-signed certificate), you can generate a standard **RSA key pair** (private and public keys) without the extra certificate-related commands.

---

## **Step 1: Generate the Private Key**
Run the following command to generate a **4096-bit RSA private key**:
```bash
openssl genpkey -algorithm RSA -out private_key.pem -pkeyopt rsa_keygen_bits:4096
```
✅ **This creates** `private_key.pem`, which contains your **private key**.

---

## **Step 2: Extract the Public Key**
Now, extract the **public key** from the private key:
```bash
openssl rsa -in private_key.pem -pubout -out public_key.pem
```
**This creates** `public_key.pem`, which contains your **public key**.

---

## **Step 3: Verify the Keys**
Check if the private key is valid:
```bash
openssl rsa -in private_key.pem -check
```
✅ **Expected output:**
```
RSA key ok
```

Check if the public key is valid:
```bash
openssl rsa -in public_key.pem -pubin -text -noout
```
✅ **Expected output:**  
A readable format of the public key.

---

## **📌 Step 4 (Optional): Encrypt the Private Key**
For better security, you can encrypt the private key with **AES-256**:
```bash
openssl genpkey -algorithm RSA -out encrypted_private_key.pem -aes256 -pass pass:YourStrongPassword -pkeyopt rsa_keygen_bits:4096
```
✅ **This will prompt for a password**, which will be needed to use the private key.

---

## **📌 Summary of Commands**
| **Step** | **Command** |
|----------|------------|
| Generate a **private key** | `openssl genpkey -algorithm RSA -out private_key.pem -pkeyopt rsa_keygen_bits:4096` |
| Extract the **public key** | `openssl rsa -in private_key.pem -pubout -out public_key.pem` |
| Verify **private key** | `openssl rsa -in private_key.pem -check` |
| Verify **public key** | `openssl rsa -in public_key.pem -pubin -text -noout` |
| Encrypt **private key (optional)** | `openssl genpkey -algorithm RSA -out encrypted_private_key.pem -aes256 -pass pass:YourStrongPassword -pkeyopt rsa_keygen_bits:4096` |

---

### **📌 Key Differences Between This and the Previous Method**
| **Feature** | **Normal Key Pair (This Method)** | **Self-Signed Certificate (Previous Method)** |
|------------|--------------------------------|--------------------------------|
| **Purpose** | Used for **encryption, signing, and authentication** | Used for **SSL/TLS authentication** |
| **Files Generated** | `private_key.pem`, `public_key.pem` | `key.pem` (private), `cert.pem` (public cert) |
| **Requires a CN (Common Name)?** | ❌ No | ✅ Yes |
| **Encrypt Data?** | ✅ Yes, encrypt/sign messages | ✅ Yes, secure web connections (HTTPS) |

---

### **📌 What Can You Do With This Key Pair?**
- **🔐 Encrypt & Decrypt Messages** using RSA.
- **🖊️ Digitally Sign & Verify Data**.
- **🔑 Authenticate Users** (SSH key-based authentication).
