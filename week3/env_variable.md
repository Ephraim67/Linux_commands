## **Environment Variables in Shell Basics**  

### **What Are Environment Variables?**  
Environment variables are key-value pairs used by the **shell** and **programs** to store system-wide settings, user preferences, and configurations. They help control system behavior and provide essential information about the environment.

---

## **1. Viewing Environment Variables**
To display all environment variables, use:
```bash
printenv
env
```
To check a specific variable:
```bash
echo $HOME
echo $USER
echo $PATH
```
> **Activity:** Run `printenv` and identify key environment variables.

---

## **2. Common Environment Variables**
| **Variable** | **Description** |
|-------------|----------------|
| `$USER` | Shows the currently logged-in user. |
| `$HOME` | Home directory of the user. |
| `$SHELL` | The shell being used (e.g., `/bin/bash`). |
| `$PWD` | Current working directory. |
| `$PATH` | Directories where the shell looks for executable files. |
| `$EDITOR` | Default text editor (e.g., `vim` or `nano`). |
| `$LANG` | System language setting. |
| `$LOGNAME` | The name of the logged-in user. |

> **Activity:** Use `echo` to display values of different environment variables.

---

## **3. Setting and Modifying Environment Variables**
### **a. Temporarily Setting a Variable**
```bash
export MY_VAR="Hello, Linux!"
echo $MY_VAR
```
This variable will **only** exist in the current terminal session.

### **b. Permanently Setting a Variable**
To make a variable **persistent**, add it to:
- **Bash users:** `~/.bashrc`
- **Zsh users:** `~/.zshrc`
- **System-wide:** `/etc/environment`

Example:
```bash
echo 'export MY_VAR="Hello, World!"' >> ~/.bashrc
source ~/.bashrc
```

> **Activity:** Set a temporary variable, then make it permanent.

---

## **4. Unsetting and Deleting Variables**
To remove an environment variable:
```bash
unset MY_VAR
echo $MY_VAR   # Will return empty
```
> **Activity:** Set a variable, use `unset`, and verify it’s removed.

---

## **5. Using Environment Variables in Scripts**
Variables are useful in **Bash scripting**.

Example script (`myscript.sh`):
```bash
#!/bin/bash
echo "Hello, $USER!"
echo "Your home directory is: $HOME"
```
Run the script:
```bash
chmod +x myscript.sh
./myscript.sh
```
> **Activity:** Create a script that uses `$USER` and `$HOME`.

---

## **6. Path and Command Execution**
The `$PATH` variable stores directories where the system looks for executable commands.

Example:
```bash
echo $PATH
```
To add a directory permanently:
```bash
export PATH=$PATH:/home/user/scripts
```
> **Activity:** Add a custom directory to `$PATH` and execute a script from it.
