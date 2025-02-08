# **Linux File Permissions**  

In Linux, file permissions are critical to securing files and controlling access to system resources. Understanding how file permissions work is essential for system administration, security, and user management.

---

## **1. What Are File Permissions?**  
File permissions determine which users can read, write, or execute a file. Each file or directory in Linux has three types of permissions:

- **Read (r)**: Permission to view the contents of the file or directory.
- **Write (w)**: Permission to modify or delete the file or add/remove files in a directory.
- **Execute (x)**: Permission to run the file as a program or script, or to access files within a directory.

---

## **2. Structure of File Permissions**  
Permissions are displayed when you run the `ls -l` command, and they follow this format:

```
-rwxr-xr-x 1 user group 1234 Jan 1 12:00 example.txt
```

- The first character represents the **file type** (e.g., `-` for regular file, `d` for directory).
- The next three characters represent the **owner’s** permissions (e.g., `rwx` for read, write, and execute).
- The next three characters represent the **group’s** permissions.
- The last three characters represent the **others’** permissions.

**Example Breakdown:**
```
-rwxr-xr-x
|   |   |
|   |   └─── Others' permissions (r-x = read and execute)
|   └────── Group permissions (r-x = read and execute)
└────────── Owner permissions (rwx = read, write, and execute)
```

---

## **3. Types of File Owners**  
Permissions are granted to three categories of users:

- **Owner**: The user who owns the file.
- **Group**: Users who belong to the same group as the file.
- **Others**: All other users on the system.

---

## **4. Changing File Permissions with `chmod`**  
The `chmod` (change mode) command is used to modify file permissions. It allows you to set permissions using either symbolic or numeric modes.

### **a. Symbolic Mode**  
Permissions can be granted or revoked using symbols:
- **r**: Read
- **w**: Write
- **x**: Execute
- **+**: Add permission
- **-**: Remove permission
- **=**: Set exact permission

#### **Examples:**
1. **Grant execute permission to the owner**:
   ```bash
   chmod u+x file.txt
   ```
   This adds **execute** permission (`+x`) to the **user (owner)** (`u`) for `file.txt`.

2. **Remove read permission for the group**:
   ```bash
   chmod g-r file.txt
   ```
   This removes **read** permission (`-r`) for the **group** (`g`).

3. **Set read and write permission for the owner only**:
   ```bash
   chmod u=rw file.txt
   ```
   This sets **read** and **write** permission (`rw`) for the **owner** (`u`), and removes all other permissions.

### **b. Numeric Mode**  
Permissions can also be set using numbers, where:
- **r = 4**
- **w = 2**
- **x = 1**
- **- = 0**

Each user category (owner, group, and others) is assigned a number. The sum of these numbers determines the permissions.

#### **Examples:**
1. **Owner has read, write, and execute permissions; group and others have read and execute**:
   ```bash
   chmod 755 file.txt
   ```
   Breakdown:
   - Owner (`7 = rwx`)
   - Group (`5 = r-x`)
   - Others (`5 = r-x`)

2. **Owner has read and write; group and others have no permissions**:
   ```bash
   chmod 600 file.txt
   ```

---

## **5. Viewing File Permissions with `ls -l`**  
You can check the current permissions of files and directories using the `ls -l` command:

```bash
ls -l file.txt
```

This will output a line showing the file's permissions, owner, group, and other metadata.

---

## **6. Changing Ownership with `chown`**  
The `chown` command changes the ownership of a file or directory.

### **Examples:**
1. **Change the owner of a file**:
   ```bash
   chown username file.txt
   ```

2. **Change both owner and group**:
   ```bash
   chown username:groupname file.txt
   ```

---

## **7. File Permissions for Directories**  
In addition to regular files, directories also have permissions, which affect what actions you can perform on them:

- **Read (r)**: View the contents of the directory (list files inside).
- **Write (w)**: Add, delete, or rename files within the directory.
- **Execute (x)**: Access files within the directory.

### **Important Note:**
- A directory **must have execute permission** for you to access any files inside it, even if the files themselves have proper read permissions.

---

## **8. Special Permissions in Linux**
### **a. SUID (Set User ID)**
When the **SUID** permission is set on an executable file, it allows the file to run with the privileges of the **owner**, not the user who executes it.

- **Example**:
  ```bash
  chmod u+s /usr/bin/passwd
  ```

### **b. SGID (Set Group ID)**
When **SGID** is set on a file, it runs with the **group’s** permissions, not the user’s.

- **Example**:
  ```bash
  chmod g+s file.txt
  ```

### **c. Sticky Bit**
When the **sticky bit** is set on a directory, only the file owner, the directory owner, or the root user can delete or rename files within the directory.

- **Example**:
  ```bash
  chmod +t /tmp
  ```

---

## **9. Practical Activities for Students**
### **Activity 1: Viewing and Modifying Permissions**
1. Use `ls -l` to view file permissions.
2. Modify the file’s permissions with `chmod` to allow reading, writing, and executing.
3. Remove the execute permission for the group.

### **Activity 2: Changing Ownership**
1. Use `chown` to change the file owner and group for a specific file.
2. Create a new file and assign it to a new user.

### **Activity 3: Using Special Permissions**
1. Set the SUID permission on a file and test it.
2. Set the sticky bit on a directory and test file deletion.
