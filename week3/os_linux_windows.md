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
