Linux command-line tasks can range from basic file management to advanced scripting and automation. Below are some essential Linux command-line tasks categorized by functionality.

---

## **1. File and Directory Management**
### **Create, List, and Navigate Directories**
| Command | Description |
|---------|------------|
| `ls` | List files in a directory |
| `ls -la` | List all files, including hidden ones, with details |
| `mkdir newdir` | Create a new directory |
| `cd newdir` | Change to the `newdir` directory |
| `pwd` | Show the current working directory |
| `rmdir emptydir` | Remove an empty directory |

### **Create, Delete, and Copy Files**
| Command | Description |
|---------|------------|
| `touch file.txt` | Create an empty file |
| `rm file.txt` | Remove a file |
| `rm -rf directory` | Force delete a directory and its contents |
| `cp file1.txt file2.txt` | Copy file1.txt to file2.txt |
| `mv oldname.txt newname.txt` | Rename or move a file |

---

## **2. File Content Viewing and Editing**
| Command | Description |
|---------|------------|
| `cat file.txt` | View file contents |
| `less file.txt` | View a file page by page |
| `head -n 10 file.txt` | Show the first 10 lines of a file |
| `tail -n 10 file.txt` | Show the last 10 lines of a file |
| `nano file.txt` | Edit a file using Nano |
| `vim file.txt` | Edit a file using Vim |

---

## **3. File Permissions and Ownership**
| Command | Description |
|---------|------------|
| `ls -l` | View file permissions |
| `chmod 755 script.sh` | Change file permissions (`rwxr-xr-x`) |
| `chown user:group file.txt` | Change file ownership |
| `sudo` | Execute a command as root |

### **Understanding Permissions**
- `r` (read), `w` (write), `x` (execute)
- Example: `chmod 644 file.txt` (Owner: `rw-`, Group: `r--`, Others: `r--`)

---

## **4. Process and Resource Management**
| Command | Description |
|---------|------------|
| `ps aux` | Show running processes |
| `top` | Display system resource usage |
| `kill PID` | Kill a process by its PID |
| `pkill processname` | Kill a process by name |
| `htop` | Interactive process viewer (if installed) |

---

## **5. User Management**
| Command | Description |
|---------|------------|
| `whoami` | Show current user |
| `id` | Display user ID and group ID |
| `adduser newuser` | Add a new user |
| `passwd newuser` | Change a user's password |
| `usermod -aG sudo newuser` | Grant sudo privileges |

---

## **6. Networking**
| Command | Description |
|---------|------------|
| `ping google.com` | Check internet connectivity |
| `ifconfig` or `ip a` | Show network interfaces |
| `curl example.com` | Fetch content from a URL |
| `wget file_url` | Download a file |
| `netstat -tulnp` | Show open ports |

---

## **7. Disk Management**
| Command | Description |
|---------|------------|
| `df -h` | Show disk usage |
| `du -sh folder/` | Show folder size |
| `mount /dev/sdb1 /mnt` | Mount a disk |
| `umount /mnt` | Unmount a disk |

---

## **8. Searching and Finding Files**
| Command | Description |
|---------|------------|
| `find / -name "file.txt"` | Search for a file by name |
| `grep "error" logfile.txt` | Search for "error" in a file |
| `grep -r "keyword" /var/log/` | Search recursively in a directory |

---

## **9. Archiving and Compression**
| Command | Description |
|---------|------------|
| `tar -cvf archive.tar folder/` | Create a tar archive |
| `tar -xvf archive.tar` | Extract a tar archive |
| `zip -r archive.zip folder/` | Create a zip file |
| `unzip archive.zip` | Extract a zip file |

---

## **10. Scripting and Automation**
### **Basic Shell Script**
Create a simple script (`myscript.sh`):
```bash
#!/bin/bash
echo "Hello, Linux!"
date
```
Make it executable:
```bash
chmod +x myscript.sh
./myscript.sh
```

---

## **11. System Information**
| Command | Description |
|---------|------------|
| `uname -a` | Show kernel information |
| `uptime` | Display system uptime |
| `free -m` | Show memory usage |
| `df -h` | Show disk usage |

---

### **Bonus: One-Liner Productivity Commands**
1. **Find and delete files larger than 100MB:**
   ```bash
   find /path/to/dir -size +100M -delete
   ```
2. **Monitor a log file in real-time:**
   ```bash
   tail -f /var/log/syslog
   ```
3. **Find top 10 largest files:**
   ```bash
   du -ah / | sort -rh | head -n 10
   ```

---
