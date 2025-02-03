# **Redirection in Shell Basics**  

In **Linux**, redirection allows you to **control where input comes from and where output goes**. By default, a command takes input from the keyboard and outputs to the terminal. However, we can **redirect** this input and output to files or other commands.

---

## **1. Understanding Input & Output Streams**  
Every process in Linux has three standard data streams:

| **Stream** | **Description** | **File Descriptor** | **Default Destination** |
|------------|----------------|---------------------|-------------------------|
| **Standard Input (stdin)** | Input to a command | `0` | Keyboard |
| **Standard Output (stdout)** | Normal output from a command | `1` | Terminal (screen) |
| **Standard Error (stderr)** | Error messages | `2` | Terminal (screen) |

---

## **2. Output Redirection (`>` and `>>`)**  
### **a. Redirecting Standard Output (`>`)**
Redirects output to a file, **overwriting** if the file exists.

#### **Example:**
```bash
ls > files.txt
```
- This stores the output of `ls` in `files.txt`, **overwriting** any previous content.

#### **Verify by reading the file:**
```bash
cat files.txt
```

---

### **b. Appending Output (`>>`)**
Redirects output to a file, but **appends** instead of overwriting.

#### **Example:**
```bash
echo "New line added" >> files.txt
```
- This **adds** `"New line added"` to `files.txt` without removing existing content.

> **Activity:** Try using `>` and `>>` to see the difference.

---

## **3. Input Redirection (`<`)**
Redirects a file’s content as input to a command.

#### **Example:**
```bash
sort < files.txt
```
- The `sort` command reads **input from `files.txt`** instead of the keyboard.

> **Activity:** Create a file with multiple lines, then use `sort < filename` to sort its contents.

---

## **4. Redirecting Error Messages (`2>` and `2>>`)**  
### **a. Redirecting Errors (`2>`)**
Redirects **stderr** to a file, overwriting it.

#### **Example:**
```bash
ls /wrongpath 2> error.log
```
- Any error messages from `ls /wrongpath` go into `error.log`.

---

### **b. Appending Errors (`2>>`)**
Appends error messages instead of overwriting.

#### **Example:**
```bash
ls /anotherwrongpath 2>> error.log
```
- Errors from `ls /anotherwrongpath` are **added** to `error.log`.

> **Activity:** Try running commands that produce errors and redirect them.

---

## **5. Redirecting Both Output & Errors (`&>` and `2>&1`)**  
### **a. Redirecting Both (`&>`)**
Redirects **stdout and stderr** to the same file.

#### **Example:**
```bash
ls /correct /wrongpath &> output.log
```

---

### **b. Redirecting Errors to Standard Output (`2>&1`)**
Redirects stderr (`2`) to stdout (`1`), then redirects everything to a file.

#### **Example:**
```bash
ls /correct /wrongpath > output.log 2>&1
```

> **Activity:** Experiment with `&>` and `2>&1` to see how errors and output are handled.

---

## **6. Piping (`|`) - Redirecting Output to Another Command**
Pipes (`|`) send output from one command as input to another.

#### **Example:**
```bash
ls | grep ".txt"
```
- The `ls` command lists files, and `grep` filters `.txt` files.

> **Activity:** Try `ps aux | grep bash` to find all running Bash processes.

---

## **7. Null Redirection (`/dev/null`) - Discarding Output**
`/dev/null` is a special file that **discards** anything written to it.

#### **Example:**
```bash
ls /wrongpath 2> /dev/null
```
- Errors are discarded and won’t show up.

> **Activity:** Try redirecting both stdout and stderr to `/dev/null`.
