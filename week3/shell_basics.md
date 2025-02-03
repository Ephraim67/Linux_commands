## **1. Understanding the Linux Shell**
The **Linux shell** is a command-line interface (CLI) that allows users to interact with the operating system. It acts as a middleman between the user and the kernel, enabling users to execute commands, automate tasks, and manage files.

### **Common Shells in Linux**
| Shell | Description |
|-------|------------|
| **Bourne Shell (sh)** | The original UNIX shell, lightweight and minimal. |
| **C Shell (csh)** | Uses C-like syntax, suitable for programmers. |
| **Korn Shell (ksh)** | An improved version of `sh` with additional scripting features. |
| **Bourne Again Shell (bash)** | The most widely used shell, with many powerful features. |

> **Activity**: Run `echo $SHELL` in the terminal to check which shell is currently in use.

---

## **2. Basic Shell Commands**
Here are some essential commands for beginners:

### **a. Navigating Directories**
- `pwd` → Print the current working directory.
- `ls` → List files and directories.
- `cd` → Change directories.

**Example:**
```bash
pwd
cd /home/user/Documents
ls -l
```

> **Activity**: Create a directory, navigate to it, and check its contents using the commands above.

---

### **b. File and Directory Management**
| Command | Description |
|---------|------------|
| `mkdir myfolder` | Create a new directory. |
| `rmdir myfolder` | Remove an empty directory. |
| `touch file.txt` | Create a new file. |
| `rm file.txt` | Delete a file. |
| `cp file.txt /tmp/` | Copy a file. |
| `mv file.txt newfile.txt` | Rename (move) a file. |

> **Activity**: Create, rename, copy, and delete files inside a directory.

---

### **c. Viewing and Editing Files**
| Command | Description |
|---------|------------|
| `cat file.txt` | View file contents. |
| `nano file.txt` | Open a file in `nano` text editor. |
| `less file.txt` | View large files one page at a time. |
| `head -n 5 file.txt` | View the first 5 lines. |
| `tail -n 5 file.txt` | View the last 5 lines. |

> **Activity**: Create a text file, add content using `nano`, and view it using `cat`, `less`, and `tail`.

---

### **d. File Permissions and Ownership**
| Command | Description |
|---------|------------|
| `ls -l` | View file permissions. |
| `chmod 755 file.sh` | Change file permissions. |
| `chown user:group file.sh` | Change file ownership. |

**Example:**
```bash
chmod +x script.sh   # Give execute permission
ls -l script.sh      # Check permissions
```

> **Activity**: Create a script, modify its permissions, and run it.

---

## **3. Introduction to Linux Scripting**
**Bash scripting** allows users to automate tasks using commands inside a script file.

**Example Script:**
```bash
#!/bin/bash
echo "Hello, $(whoami)! Welcome to Linux."
date
```
Save this script as `hello.sh`, then run:
```bash
chmod +x hello.sh
./hello.sh
```

> **Activity**: Write and execute a simple script that prints "Hello, Linux!"

---

## **4. Advanced Shell Features**
### **a. Redirection & Piping**
- `>` → Redirect output to a file.
- `>>` → Append output to a file.
- `|` → Pipe output from one command to another.

**Example:**
```bash
ls -l > file_list.txt   # Save output to a file
cat file_list.txt | grep ".txt"  # Find files with ".txt" in their name
```

> **Activity**: Use redirection and pipes to filter and save command output.

---

### **b. Process Management**
- `ps` → Show running processes.
- `top` → Monitor system resources.
- `kill <PID>` → Terminate a process.
- `htop` → Interactive process manager (if installed).

**Example:**
```bash
ps aux | grep firefox  # Find Firefox process
kill <PID>  # Replace <PID> with the actual process ID
```

> **Activity**: List running processes and terminate a selected process.
