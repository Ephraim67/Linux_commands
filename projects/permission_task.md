### 📝 **Task: File Permission Mastery**

**Objective:** Learn how to manage and apply file permissions using `chmod`.

#### Setup:

1. Open your terminal.
2. Create a new directory called `permissions_lab`.
3. Inside it, create three files: `script.sh`, `report.txt`, and `data.db`.

```bash
mkdir permissions_lab
cd permissions_lab
touch script.sh report.txt data.db
```

#### Task Instructions:

1. **Initial Permissions Check:**

   * Use `ls -l` to list the permissions of all files.
   * Write down or screenshot the output.

2. **Assign Permissions:**

   * Give full permissions (read, write, execute) to all users for `script.sh` using **numeric mode**.
   * Give read and write permissions to the **owner**, read-only to the **group**, and no permissions to **others** for `report.txt` using **symbolic mode**.
   * Remove all permissions from `data.db`.

3. **Commands to Use:**

   * `chmod 777 script.sh`
   * `chmod u=rw,g=r,o= report.txt`
   * `chmod 000 data.db`

4. **Verification:**

   * Run `ls -l` again to confirm the permission changes.
   * Note any differences and explain what each set of permissions means.

5. **Bonus (Challenge):**

   * Create a subdirectory `testdir` inside `permissions_lab`.
   * Set full permissions to `testdir` **recursively** (for the folder and its content).
   * Command hint: `chmod -R 777 testdir`


### Questions:

1. What does the number 7 represent in `chmod 777`?
2. Why is it dangerous to give full permissions to all users on a script?
3. What happens when you try to open `data.db` after removing all permissions?
