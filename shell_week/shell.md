## **1. Create and Run Your First Bash Shell Script**
### **Steps:**
1. Create a script file:
   ```bash
   nano myscript.sh
   ```
2. Add the following content:
   ```bash
   #!/bin/bash
   echo "Hello, World!"
   ```
3. Make it executable:
   ```bash
   chmod +x myscript.sh
   ```
4. Run the script:
   ```bash
   ./myscript.sh
   ```
---

## **2. Understanding Variables in Bash Shell Scripting**
### **Types of Variables:**
- **User-defined variables**
  ```bash
  name="Kali"
  echo "Hello, $name"
  ```
- **Environment variables**
  ```bash
  echo "Current User: $USER"
  echo "Home Directory: $HOME"
  ```
- **Read-only variables**
  ```bash
  readonly myvar="This is constant"
  ```
---

## **3. Passing Arguments to Bash Scripts**
### **Example:**
```bash
#!/bin/bash
echo "Script name: $0"
echo "First argument: $1"
echo "Second argument: $2"
echo "All arguments: $@"
echo "Total arguments: $#"
```
### **Run the script:**
```bash
./myscript.sh Kali Linux
```

---

## **4. Using Arrays in Bash**
### **Example:**
```bash
#!/bin/bash
fruits=("Apple" "Banana" "Cherry")

# Print all elements
echo "All Fruits: ${fruits[@]}"

# Print specific element
echo "First Fruit: ${fruits[0]}"

# Add element
fruits+=("Mango")

# Length of array
echo "Total Fruits: ${#fruits[@]}"
```

---

## **5. Using Arithmetic Operators in Bash Scripting**
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

---

## **6. String Operations in Bash**
### **Example:**
```bash
#!/bin/bash
str="Hello Kali"

# String Length
echo "Length: ${#str}"

# Substring
echo "Substring: ${str:6:4}"

# Replace substring
echo "Replace: ${str/Kali/Linux}"
```

---

## **7. Decision Making With If Else and Case Statements**
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
### **Case Statement Example:**
```bash
#!/bin/bash
read -p "Enter a fruit name: " fruit

case $fruit in
  "Apple") echo "You chose Apple." ;;
  "Banana") echo "You chose Banana." ;;
  "Cherry") echo "You chose Cherry." ;;
  *) echo "Unknown fruit!" ;;
esac
```

---

## **8. Loops in Bash**
### **For Loop:**
```bash
#!/bin/bash
for i in {1..5}; do
  echo "Iteration $i"
done
```
### **While Loop:**
```bash
#!/bin/bash
count=1

while [ $count -le 5 ]; do
  echo "Count: $count"
  ((count++))
done
```
### **Until Loop:**
```bash
#!/bin/bash
num=1

until [ $num -ge 5 ]; do
  echo "Number: $num"
  ((num++))
done
```
