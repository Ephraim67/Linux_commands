# **Copying and Renaming Files in Linux**

## **1. Introduction**
Managing files efficiently is a core skill in Linux. Copying and renaming files are essential tasks that allow users to back up data, organize files, and automate workflows. The primary commands used for these operations are:
- `cp` (copy)
- `mv` (move/rename)

These commands work across different directories and file systems and can be combined with other commands for automation.

---

## **2. Copying Files with `cp`**
The `cp` command copies files and directories.

### **Basic Syntax**
```bash
cp [options] source destination
```
- `source` – The file or directory to copy.
- `destination` – The target location where the file is copied.

---

### **Basic Copy Example**
#### **Copy a Single File**
```bash
cp file1.txt file2.txt
```
This copies `file1.txt` to `file2.txt` in the same directory.

#### **Copy a File to Another Directory**
```bash
cp file1.txt /home/user/Documents/
```
Copies `file1.txt` to the `Documents/` directory.

#### **Copy a File with a New Name**
```bash
cp file1.txt /home/user/Documents/newfile.txt
```
Copies `file1.txt` to `Documents/` and renames it to `newfile.txt`.

---

### **Copying Multiple Files**
#### **Copy Multiple Files to a Directory**
```bash
cp file1.txt file2.txt file3.txt /home/user/Documents/
```
Copies all three files to the `Documents/` directory.

#### **Copy All Files in a Directory (`*` wildcard)**
```bash
cp *.txt /home/user/Documents/
```
Copies all `.txt` files to the `Documents/` directory.

---

### **Copying Directories**
By default, `cp` does not copy directories. Use the `-r` (recursive) option to copy directories.

#### **Copy a Directory and Its Contents**
```bash
cp -r folder1 /home/user/Documents/
```
Copies `folder1` and all its files/subdirectories into `Documents/`.

---

### **Preserving File Attributes**
Use the `-p` option to retain timestamps, permissions, and ownership.

```bash
cp -p file1.txt /home/user/Documents/
```

Use `-a` (archive mode) for full preservation (equivalent to `-rp`).
```bash
cp -a folder1 /home/user/Documents/
```

---

### **Overwriting Protection**
To prevent accidental overwrites, use `-i` (interactive mode).
```bash
cp -i file1.txt /home/user/Documents/
```
It prompts before overwriting an existing file.

To **force overwriting** without confirmation:
```bash
cp -f file1.txt /home/user/Documents/
```

---

### **Verifying Copy Progress**
To see progress when copying large files, use the `-v` (verbose) option.
```bash
cp -v file1.txt /home/user/Documents/
```
Displays messages about what’s being copied.

---

## **3. Renaming and Moving Files with `mv`**
The `mv` command is used for both **moving** and **renaming** files.

### **Basic Syntax**
```bash
mv [options] source destination
```
- `source` – The file or directory to move or rename.
- `destination` – The new name or location.

---

### **Renaming Files**
```bash
mv oldname.txt newname.txt
```
Renames `oldname.txt` to `newname.txt`.

---

### **Moving Files to Another Directory**
```bash
mv file1.txt /home/user/Documents/
```
Moves `file1.txt` into `Documents/`.

---

### **Renaming While Moving**
```bash
mv file1.txt /home/user/Documents/newfile.txt
```
Moves `file1.txt` to `Documents/` and renames it to `newfile.txt`.

---

### **Moving Multiple Files**
```bash
mv file1.txt file2.txt file3.txt /home/user/Documents/
```
Moves all three files into `Documents/`.

---

### **Renaming Directories**
```bash
mv old_folder new_folder
```
Renames `old_folder` to `new_folder`.

---

### **Moving a Directory**
```bash
mv folder1 /home/user/Documents/
```
Moves `folder1` into `Documents/`.

---

### **Prevent Overwriting**
To avoid accidental overwrites, use the `-i` option.
```bash
mv -i file1.txt /home/user/Documents/
```
Prompts before overwriting an existing file.

To force overwrite:
```bash
mv -f file1.txt /home/user/Documents/
```

---

## **4. Advanced Techniques**
### **Using Wildcards for Multiple File Operations**
- Move all `.txt` files:
  ```bash
  mv *.txt /home/user/Documents/
  ```
- Rename all `.jpg` files to `.png`:
  ```bash
  for file in *.jpg; do mv "$file" "${file%.jpg}.png"; done
  ```

---

### **Using `find` and `xargs` for Bulk Copying**
Copy all `.txt` files from one directory to another:
```bash
find /source/path -name "*.txt" | xargs -I {} cp {} /destination/path/
```

---

### **Using `rsync` for Efficient Copying**
Instead of `cp`, use `rsync` for large file transfers:
```bash
rsync -av source_directory/ destination_directory/
```
- `-a` (archive mode) preserves permissions and timestamps.
- `-v` (verbose) shows progress.

---

## **5. Practical Activities for Students**
### **Activity 1: Basic File Copying**
1. Create a new directory called `test_files`.
2. Create three text files inside `test_files`.
3. Copy one file to another name.
4. Copy all three files to a new directory.

### **Activity 2: Copying and Renaming Directories**
1. Create a directory named `backup`.
2. Copy `test_files/` into `backup/`.
3. Rename `backup/` to `final_backup/`.

### **Activity 3: Using Wildcards**
1. Create multiple `.txt` files in a directory.
2. Move all `.txt` files to a `Text_Files/` directory.
3. Rename all `.txt` files to `.bak`.

### **Activity 4: Automating File Copies with `cron`**
1. Set up a `cron` job to back up the `Documents/` folder every hour.
   ```bash
   crontab -e
   ```
2. Add the following line:
   ```bash
   0 * * * * cp -r /home/user/Documents/ /home/user/Backup/
   ```
3. Verify that the backup occurs every hour.

---

## **6. Summary**
- `cp` is used for copying files and directories.
- `mv` is used for renaming and moving files.
- Wildcards (`*`) allow copying/moving multiple files at once.
- `rsync` is more efficient for large file transfers.
- `cron` automates backup tasks.
