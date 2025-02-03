## **Basics of Windows and Linux Operating Systems**
### **1. Overview**
- **Windows OS:** A graphical user interface (GUI)-based operating system developed by Microsoft.
- **Linux OS:** An open-source, Unix-like OS that comes in multiple distributions (Ubuntu, CentOS, Debian, etc.).

### **2. Key Differences**
| Feature       | Windows       | Linux |
|--------------|--------------|-------|
| **Interface** | GUI-based (with CLI options like PowerShell & CMD) | CLI-based with GUI options |
| **File System** | NTFS, FAT32 | ext4, xfs, btrfs |
| **User Management** | Admin & Standard Users | Root & Regular Users |
| **Customization** | Limited | Highly customizable |
| **Software** | Proprietary | Mostly Open-source |

---

## **Common File Structures and Permissions**
### **1. File System Structure**
#### **Windows**
- **C:\** (Root Directory)
  - **Program Files/** (Installed software)
  - **Users/** (User accounts)
  - **Windows/** (OS files)
  - **System32/** (Critical system files)

#### **Linux**
- **/** (Root Directory)
  - **/bin/** (Essential binaries)
  - **/home/** (User directories)
  - **/etc/** (Configuration files)
  - **/var/** (Log files)
  - **/usr/** (User-installed applications)

### **2. File Permissions**
#### **Windows Permissions (NTFS)**
- Read (R), Write (W), Execute (X)
- User groups: Admins, Users, System

#### **Linux File Permissions**
- **Format:** `rwxr-xr--`
  - **User (Owner)**: rwx (Read, Write, Execute)
  - **Group**: r-x (Read, Execute)
  - **Others**: r-- (Read only)
- **Changing Permissions:**
  - `chmod 755 file.txt` (Sets user full control, group & others read/execute)
  - `chown user:group file.txt` (Changes ownership)

---

## **Introduction to the Command Line**
### **1. Windows (PowerShell & CMD)**
- **Basic Commands:**
  - `dir` (List files)
  - `cd <folder>` (Change directory)
  - `copy <file> <destination>` (Copy files)
  - `del <file>` (Delete file)
  - `Get-Help <command>` (Get help for a command)

### **2. Linux (Bash Terminal)**
- **Basic Commands:**
  - `ls` (List files)
  - `cd <folder>` (Change directory)
  - `cp file.txt /home/user/` (Copy files)
  - `rm file.txt` (Delete file)
  - `man <command>` (Command help)

---

## **Activities**
### **1. Practice File Manipulation & Permissions**
- **Windows:**
  - Create a file using PowerShell: `New-Item -ItemType File -Name "test.txt"`
  - Change permissions: `icacls test.txt /grant Everyone:R`
  
- **Linux:**
  - Create a file: `touch test.txt`
  - Change permissions: `chmod 644 test.txt`
  - Change ownership: `chown user:group test.txt`

### **2. Basic Scripting for Automation**
#### **Windows PowerShell Script**
```powershell
# Create a backup script
Copy-Item -Path "C:\Users\Documents\*" -Destination "D:\Backup\" -Recurse
```
#### **Linux Bash Script**
```bash
#!/bin/bash
# Backup home directory
cp -r /home/user/ /backup/
```
- **Execution:**
  - **Windows:** Run `backup.ps1` in PowerShell.
  - **Linux:** Run `chmod +x backup.sh && ./backup.sh`.


--------------------------------------------------------------------------------------------------------------------------------

## **1. Practice File Manipulation & Permissions**

### **Windows (PowerShell & CMD) Exercises**
#### **Exercise 1: Creating and Managing Files**
1. Open PowerShell and create a directory called `TestFolder`:
   ```powershell
   New-Item -Path "C:\Users\Public\TestFolder" -ItemType Directory
   ```
2. Navigate to the folder:
   ```powershell
   cd C:\Users\Public\TestFolder
   ```
3. Create a text file:
   ```powershell
   New-Item -Name "sample.txt" -ItemType File
   ```
4. Write text to the file:
   ```powershell
   Set-Content -Path "sample.txt" -Value "Hello, this is a test file!"
   ```
5. Append new content to the file:
   ```powershell
   Add-Content -Path "sample.txt" -Value "Appending new text."
   ```
6. Display the file’s contents:
   ```powershell
   Get-Content -Path "sample.txt"
   ```
7. Delete the file:
   ```powershell
   Remove-Item -Path "sample.txt"
   ```

#### **Exercise 2: Managing Permissions**
1. Create a new file:
   ```powershell
   New-Item -Name "secure.txt" -ItemType File
   ```
2. View the file’s permissions:
   ```powershell
   Get-Acl -Path "secure.txt"
   ```
3. Grant read-only access to all users:
   ```powershell
   icacls secure.txt /grant Everyone:R
   ```
4. Remove all permissions except for the owner:
   ```powershell
   icacls secure.txt /inheritance:r /remove Everyone
   ```

---

### **Linux (Bash Terminal) Exercises**
#### **Exercise 1: Creating and Managing Files**
1. Open a terminal and create a new directory:
   ```bash
   mkdir ~/TestFolder
   ```
2. Navigate to the folder:
   ```bash
   cd ~/TestFolder
   ```
3. Create a new text file:
   ```bash
   touch sample.txt
   ```
4. Write text into the file:
   ```bash
   echo "Hello, this is a test file!" > sample.txt
   ```
5. Append text to the file:
   ```bash
   echo "Appending new text." >> sample.txt
   ```
6. Display the file’s contents:
   ```bash
   cat sample.txt
   ```
7. Delete the file:
   ```bash
   rm sample.txt
   ```

#### **Exercise 2: Managing Permissions**
1. Create a new file:
   ```bash
   touch secure.txt
   ```
2. View the file’s permissions:
   ```bash
   ls -l secure.txt
   ```
3. Make the file readable and writable only by the owner:
   ```bash
   chmod 600 secure.txt
   ```
4. Make the file executable for all users:
   ```bash
   chmod 755 secure.txt
   ```
5. Change ownership of the file (requires sudo):
   ```bash
   sudo chown username:groupname secure.txt
   ```
   *(Replace `username` with an actual user and `groupname` with a valid group.)*

---

## **2. Basic Scripting for Automation**

### **Windows PowerShell Script**
**Scenario:** Automatically backup important documents.

1. Open Notepad and enter the following script:
   ```powershell
   # Backup script
   $source = "C:\Users\Public\Documents\"
   $destination = "D:\Backup\"
   Copy-Item -Path $source -Destination $destination -Recurse
   ```
2. Save it as `backup.ps1`.
3. Run the script:
   ```powershell
   powershell -ExecutionPolicy Bypass -File C:\Path\To\backup.ps1
   ```

**Challenge:** Modify the script to rename backups with timestamps.

---

### **Linux Bash Script**
**Scenario:** Automatically archive log files.

1. Open a terminal and create a new script:
   ```bash
   nano backup.sh
   ```
2. Enter the following script:
   ```bash
   #!/bin/bash
   # Backup script
   tar -czvf ~/backup_$(date +%Y%m%d).tar.gz /var/log/
   ```
3. Save and exit (`CTRL+X`, then `Y`, then `Enter`).
4. Make it executable:
   ```bash
   chmod +x backup.sh
   ```
5. Run the script:
   ```bash
   ./backup.sh
   ```

**Challenge:** Modify the script to delete backups older than 7 days:
```bash
find ~/ -name "backup_*.tar.gz" -mtime +7 -exec rm {} \;
```

---

## **3. Additional Hands-On Activities**
### **Activity 1: Find and Analyze System Information**
- **Windows:** Run `systeminfo` in PowerShell or Command Prompt.
- **Linux:** Run `uname -a` and `lsb_release -a`.

### **Activity 2: Monitor System Processes**
- **Windows:** Run `Get-Process` in PowerShell.
- **Linux:** Run `ps aux | less` in the terminal.

### **Activity 3: Automate User Account Management**
- **Windows:** Create a new user using PowerShell:
   ```powershell
   New-LocalUser -Name "student" -Password (ConvertTo-SecureString "P@ssword123" -AsPlainText -Force) -FullName "Student Account" -Description "Test account"
   ```
- **Linux:** Create a new user and set a password:
   ```bash
   sudo adduser student
   sudo passwd student
   ```
