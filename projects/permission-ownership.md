## 📁 1. Viewing File Permissions

Use the `ls -l` command:

```bash
ls -l filename
```

### Example:

```bash
-rwxr-xr-- 1 root root  1234 Jun 19 13:00 script.sh
```

### Breakdown:

* `-rwxr-xr--` → Permissions
* `1` → Link count
* `root` → Owner
* `root` → Group
* `1234` → Size in bytes
* `Jun 19 13:00` → Timestamp
* `script.sh` → File name

---

## 🔐 2. Understanding Permissions

Format: `[File Type][Owner][Group][Others]`

| Symbol | Meaning       |
| ------ | ------------- |
| `-`    | Regular file  |
| `d`    | Directory     |
| `r`    | Read          |
| `w`    | Write         |
| `x`    | Execute       |
| `-`    | No permission |

Example:

```
-rwxr-xr-- 
|   | | |
|   | | └── Others: r (read), - (no write), - (no execute)
|   | └──── Group: r (read), - (no write), x (execute)
|   └────── Owner: r (read), w (write), x (execute)
```

---

## 🛠️ 3. Changing Permissions – `chmod`

### Syntax:

```bash
chmod [permissions] filename
```

### A. **Symbolic Mode:**

```bash
chmod u+x script.sh     # Add execute to user
chmod g-w file.txt      # Remove write from group
chmod o=r file.txt      # Set others to read-only
```

### B. **Octal Mode:**

```bash
chmod 755 script.sh     # rwxr-xr-x
chmod 644 file.txt      # rw-r--r--
chmod 700 secret.txt    # rwx------
```

| Number | Permission | Meaning        |
| ------ | ---------- | -------------- |
| 7      | rwx        | Full access    |
| 6      | rw-        | Read + write   |
| 5      | r-x        | Read + execute |
| 4      | r--        | Read only      |
| 0      | ---        | No permission  |

---

## 👤 4. Changing Ownership – `chown` and `chgrp`

### Change owner:

```bash
sudo chown newuser file.txt
```

### Change owner and group:

```bash
sudo chown newuser:newgroup file.txt
```

### Change group only:

```bash
sudo chgrp newgroup file.txt
```


##  5. Examples for Practice

### A. Make a script executable by owner only:

```bash
chmod 700 exploit.sh
```

### B. Give read/write to owner and read to everyone else:

```bash
chmod 644 report.txt
```

### C. Change owner to `kali` and group to `pentesters`:

```bash
sudo chown kali:pentesters shell.py
```



##  Bonus: Recursively Change Permissions

* All files in a directory:

```bash
chmod -R 644 /path/to/files/
```

* All directories (and execute bit):

```bash
find /path -type d -exec chmod 755 {} \;
```

