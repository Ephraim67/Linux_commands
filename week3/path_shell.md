## **Command Path in Shell Basics**  

The **command path** in Linux determines where the system looks for executable files when a user types a command in the terminal. Understanding the **command path** is essential for running programs and scripts efficiently.

---

## **1. Understanding `$PATH` Variable**  
The `$PATH` variable is an environment variable that stores directories where the system searches for executable commands.

### **Check Your Current Path**
```bash
echo $PATH
```
This will output a list of directories separated by colons (`:`), for example:
```
/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/home/user/bin
```
Each directory listed is where the shell looks for commands when you type them.

> **Activity:** Run `echo $PATH` and identify key directories.

---

## **2. Finding the Location of a Command**  
To find where a command is located, use:

### **Which Command**
```bash
which ls
which python
which nano
```
This shows the **exact path** of an executable.

### **Whereis Command**
```bash
whereis ls
whereis bash
```
It provides more details, including source and documentation locations.

> **Activity:** Use `which` and `whereis` to check the paths of different commands.

---

## **3. Running Programs Using Absolute & Relative Paths**  

### **a. Absolute Path (Full Path)**
An absolute path specifies the full location of a file or program.

Example:
```bash
/bin/ls
/usr/bin/python3
```
No matter which directory you're in, this will work.

### **b. Relative Path (Relative to Current Directory)**
A relative path depends on the current working directory.

Example:
```bash
./script.sh   # Runs script from current directory
../script.sh  # Runs script from parent directory
```

> **Activity:** Create a script, run it using absolute and relative paths.

---

## **4. Modifying the `$PATH` Variable**  

### **a. Temporarily Adding a Directory to `$PATH`**
```bash
export PATH=$PATH:/home/user/scripts
```
Now, any script in `/home/user/scripts` can be run without specifying its path.

### **b. Permanently Adding a Directory to `$PATH`**
To make it permanent, add the above line to:
- **Bash users:** `~/.bashrc`  
- **Zsh users:** `~/.zshrc`  
- **System-wide:** `/etc/profile`

Example:
```bash
echo 'export PATH=$PATH:/home/user/scripts' >> ~/.bashrc
source ~/.bashrc
```

> **Activity:** Add a directory to your `$PATH` and run a script from it.

---

## **5. Executing a Script Without `./`**
Normally, a script needs `./` to execute:
```bash
./myscript.sh
```
But if it's in `$PATH`, you can run it directly:
```bash
myscript.sh
```
> **Activity:** Add a script directory to `$PATH` and execute a script without `./`.
