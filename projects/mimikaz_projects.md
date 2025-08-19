# Lab Assignment: Excluding a Folder from Antivirus Scanning & Running Mimikatz

### **Background**

Malware often uses different methods to evade antivirus (AV) detection. One simple trick is storing and running programs from folders excluded from AV scanning. Security software won’t scan excluded folders, making it possible for malicious tools to run undetected.

In this lab, you will demonstrate this concept by excluding a folder from AV scanning and executing **Mimikatz** (a well-known post-exploitation tool).

Mimikatz is a powerful tool used by attackers to steal passwords. You must only use it in your **controlled lab VM** and **never on real or production systems**.



### **Learning Objectives**

1. Understand how antivirus exclusions work.
2. Practice configuring AV exclusions on Windows.
3. Demonstrate how attackers can abuse exclusions to run tools like Mimikatz.



### **Specifications**

#### 1. Goal

* Exclude a folder from AV scanning.
* Show that Mimikatz is blocked outside the excluded folder.
* Run Mimikatz successfully inside the excluded folder.



#### 2. Lab Setup

* Windows Virtual Machine (Windows 10/11, or Windows Server)
* Antivirus enabled (Windows Defender recommended)
* Administrative privileges on the VM
* Download Mimikatz from its **official GitHub repository**:
  🔗 [https://github.com/gentilkiwi/mimikatz](https://github.com/gentilkiwi/mimikatz)



#### 3. Configure Folder Exclusion

1. Create a folder (example: `C:\LabExcluded`)
2. Open **Windows Security** → **Virus & threat protection**
3. Under **Virus & threat protection settings**, click **Manage settings**
4. Scroll to **Exclusions → Add or remove exclusions**
5. Add your folder (`C:\LabExcluded`)



#### 4. Exercise Steps

1. **Download Mimikatz**

   * Try to extract or run Mimikatz from **Downloads folder** (or Desktop).
   * Windows Defender should block it.
   * Screenshot this.

2. **Move to Excluded Folder**

   * Place Mimikatz inside your `C:\LabExcluded` folder.
   * Run it from inside this excluded folder.
   * This time, Mimikatz should run successfully.

3. **Demonstrate Execution**

   * Launch `mimikatz.exe`
   * Run a harmless command inside Mimikatz (e.g., `version` or `help`) just to prove execution.
   * Do **NOT** run password-dumping commands for this exercise.



#### 5. Validation

* ✅ Show that Mimikatz is blocked outside the excluded folder.
* ✅ Show that Mimikatz runs successfully inside the excluded folder.
* ✅ Provide screenshots (or a video) of both cases.


#### 6. Submission Requirements

* A **short report** (1–2 pages) including:

  * Steps you followed.
  * Screenshots showing folder exclusion, failed execution, and successful execution.
  * A brief explanation: *“Why excluding folders from AV scanning is risky and how attackers abuse this technique.”*

* A **video recording** (optional, for extra credit):

  * Show exclusion setup.
  * Show Defender blocking Mimikatz outside excluded folder.
  * Show successful execution inside excluded folder.
