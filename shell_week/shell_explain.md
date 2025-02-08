## **1. Create and Run Your First Bash Shell Script**
### **Explanation:**
A Bash script is a plain text file that contains a series of commands. Instead of typing commands one by one in the terminal, you can write them in a script and execute them.

### **Steps:**
1. **Create a new script file**  
   ```bash
   nano myscript.sh
   ```
2. **Write the script**  
   ```bash
   #!/bin/bash   # This tells the system to use Bash shell
   echo "Hello, World!"  # Prints text to the terminal
   ```
3. **Make the script executable**  
   ```bash
   chmod +x myscript.sh
   ```
4. **Run the script**  
   ```bash
   ./myscript.sh
   ```

---

## **2. Understanding Variables in Bash Shell Scripting**
### **Explanation:**
Variables store data that can be reused in a script. There are **two types**:
- **User-defined variables** (created by the user)
- **System environment variables** (predefined by the OS)

### **Examples:**
```bash
#!/bin/bash
name="Kali Linux"  # Defining a variable
echo "Hello, $name"  # Using the variable
```
- `$name`: Retrieves the value of the variable.
- Variables are case-sensitive (`name` and `NAME` are different).
- No spaces before/after `=` when assigning values.

---

## **3. Passing Arguments to Bash Scripts**
### **Explanation:**
You can pass arguments to a script when running it. Bash provides special variables to access these arguments:
- `$0` → Name of the script
- `$1, $2, $3, ...` → First, second, third argument, etc.
- `$@` → All arguments
- `$#` → Number of arguments

### **Example Script:**
```bash
#!/bin/bash
echo "Script Name: $0"
echo "First Argument: $1"
echo "Second Argument: $2"
echo "All Arguments: $@"
echo "Total Arguments: $#"
```
### **Running the script:**
```bash
./myscript.sh Linux Bash
```
### **Output:**
```
Script Name: ./myscript.sh
First Argument: Linux
Second Argument: Bash
All Arguments: Linux Bash
Total Arguments: 2
```

---

## **4. Using Arrays in Bash**
### **Explanation:**
Arrays store multiple values in a single variable.

### **Example:**
```bash
#!/bin/bash
fruits=("Apple" "Banana" "Cherry")

# Accessing elements
echo "First Fruit: ${fruits[0]}"

# Printing all elements
echo "All Fruits: ${fruits[@]}"

# Adding an element
fruits+=("Mango")

# Length of the array
echo "Total Fruits: ${#fruits[@]}"
```
### **Output:**
```
First Fruit: Apple
All Fruits: Apple Banana Cherry
Total Fruits: 4
```

---

## **5. Using Arithmetic Operators in Bash Scripting**
### **Explanation:**
Bash supports arithmetic operations using `(( ))` or `expr`.

### **Example:**
```bash
#!/bin/bash
a=10
b=5

echo "Addition: $((a + b))"
echo "Subtraction: $((a - b))"
echo "Multiplication: $((a * b))"
echo "Division: $((a / b))"
echo "Modulus: $((a % b))"
```
### **Output:**
```
Addition: 15
Subtraction: 5
Multiplication: 50
Division: 2
Modulus: 0
```
---

## **6. String Operations in Bash**
### **Explanation:**
Bash provides built-in string manipulation functions.

### **Example:**
```bash
#!/bin/bash
str="Hello Kali"

# String Length
echo "Length: ${#str}"

# Extract substring
echo "Substring: ${str:6:4}"  # Starts from index 6, extracts 4 chars

# Replace substring
echo "Replace: ${str/Kali/Linux}"
```
### **Output:**
```
Length: 10
Substring: Kali
Replace: Hello Linux
```

---

## **7. Decision Making With If Else and Case Statements**
### **Explanation:**
Decision-making statements allow a script to execute different commands based on conditions.

### **If-Else Example:**
```bash
#!/bin/bash
num=10

if [ $num -gt 5 ]; then
  echo "Number is greater than 5"
else
  echo "Number is 5 or less"
fi
```
### **Explanation:**
- `[ $num -gt 5 ]` → Checks if `num` is greater than 5.
- `-gt` means "greater than" (other operators: `-lt` for "less than", `-eq` for "equal", etc.).

### **Case Statement Example:**
```bash
#!/bin/bash
read -p "Enter a fruit name: " fruit

case $fruit in
  "Apple") echo "You chose Apple." ;;
  "Banana") echo "You chose Banana." ;;
  "Cherry") echo "You chose Cherry." ;;
  *) echo "Unknown fruit!" ;;  # Default case
esac
```
### **Explanation:**
- `case` is used for multiple conditions.
- `*` is the default case (like `else`).

---

## **8. Loops in Bash**
### **Explanation:**
Loops execute a block of code multiple times.

### **For Loop Example:**
```bash
#!/bin/bash
for i in {1..5}; do
  echo "Iteration $i"
done
```
### **Explanation:**
- Loops from `1` to `5` and prints each iteration.

### **While Loop Example:**
```bash
#!/bin/bash
count=1

while [ $count -le 5 ]; do
  echo "Count: $count"
  ((count++))
done
```
### **Explanation:**
- Runs while `count ≤ 5`, increasing `count` each time.

### **Until Loop Example:**
```bash
#!/bin/bash
num=1

until [ $num -ge 5 ]; do
  echo "Number: $num"
  ((num++))
done
```
### **Explanation:**
- Works like `while`, but runs **until** the condition is **true**.

---

## **Summary Table**
| Topic                          | Purpose |
|--------------------------------|---------|
| **Creating a script** | Automate tasks with Bash scripts. |
| **Variables** | Store and reuse data in a script. |
| **Arguments** | Pass input to a script when running it. |
| **Arrays** | Store multiple values in a variable. |
| **Arithmetic operations** | Perform mathematical calculations. |
| **String operations** | Manipulate text values. |
| **If-Else and Case** | Make decisions in scripts. |
| **Loops** | Repeat a block of code multiple times. |

---
