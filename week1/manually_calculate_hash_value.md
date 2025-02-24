### **🔹 How to Manually Calculate a Hashed Value & Identify Hash Corruption**  

A **hash function** takes an input and produces a fixed-size output (digest), ensuring data integrity. If even **one bit** of the input changes, the hash changes drastically.

---

## **📌 Manually Calculating a Hash**
You can manually compute a hash using **mathematical steps** or **hashing tools** like OpenSSL, Python, or Linux commands.

### **🔹 1️⃣ Using OpenSSL (SHA-256)**
Run this command in a terminal:
```bash
openssl dgst -sha256 file.txt
```
✅ **Example Output:**
```
SHA256(file.txt)= e3b0c44298fc1c149afbf4c8996fb92427ae41e4649b934ca495991b7852b855
```
This is the hash of `file.txt`.

---

### **🔹 2️⃣ Using Python (SHA-256)**
```python
import hashlib

# Read file and calculate SHA-256 hash
with open("file.txt", "rb") as f:
    file_data = f.read()
    hash_value = hashlib.sha256(file_data).hexdigest()

print("SHA-256 Hash:", hash_value)
```
✅ **Output Example:**
```
SHA-256 Hash: e3b0c44298fc1c149afbf4c8996fb92427ae41e4649b934ca495991b7852b855
```

---

### **🔹 3️⃣ Manually Calculating Hash (Conceptual)**
A simple **modular hashing function** example:
```
hash = (sum of ASCII values of characters) % 256
```
For `"hello"`:
```
h (104) + e (101) + l (108) + l (108) + o (111) = 532
532 % 256 = 20  → Hash value = 20
```
🔹 **Note:** Real-world hashing (SHA, MD5) involves **bitwise operations, padding, and compression functions**.

---

## **📌 How to Identify a Corrupt Hash**
### **🔹 1️⃣ Compare Hash Before & After Transfer**
1. Generate the **original file hash**:
   ```bash
   sha256sum file.txt
   ```
2. Transfer the file.
3. Generate the **new hash**:
   ```bash
   sha256sum file_received.txt
   ```
4. Compare both hashes.
   - ✅ **If they match** → File is intact.
   - ❌ **If different** → File is corrupted or tampered.

---

### **🔹 2️⃣ Detect Hash Corruption in Digital Signatures**
If a digital signature is based on a corrupted hash, it will **fail verification**.
```bash
openssl dgst -sha256 -verify public_key.pem -signature file.sig file.txt
```
✅ **Valid Signature Output:**
```
Verified OK
```
❌ **Corrupted Signature Output:**
```
Verification Failure
```

---

### **🔹 3️⃣ Check File Integrity with `diff`**
Use `diff` to check differences in files:
```bash
diff file.txt file_received.txt
```
If output shows **differences**, the file is corrupt.

---

## **🔹 Summary**
| **Action** | **Command** |
|------------|------------|
| Generate hash (SHA-256) | `sha256sum file.txt` |
| Compare hashes | `sha256sum file1.txt file2.txt` |
| Verify digital signature | `openssl dgst -sha256 -verify` |
| Check file corruption | `diff file1.txt file2.txt` |
