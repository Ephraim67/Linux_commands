## **Beginner Tasks**
1. **Navigation & File Management**
   - Create a directory called `my_project` inside the home folder.
   - Inside `my_project`, create three text files: `file1.txt`, `file2.txt`, and `file3.txt`.
   - Write "Hello World" into `file1.txt` using the `echo` command.
   - Copy `file1.txt` to `file1_backup.txt`.
   - Move `file2.txt` to a new directory called `backup`.

2. **File Permissions & Ownership**
   - Change permissions of `file1.txt` to **read & write for the owner, read for others**.
   - Change the owner of `file1_backup.txt` to another user.
   - Set `file3.txt` to be **executable** for all users.

3. **Viewing and Editing Files**
   - Use `nano` or `vim` to edit `file1.txt` and add some text.
   - Use `cat` to display the content of `file1.txt`.
   - Use `head` to show the first 5 lines of `file1.txt`.
   - Use `tail` to show the last 5 lines of `file1.txt`.

4. **Searching for Files & Text**
   - Find all `.txt` files inside `my_project`.
   - Search for the word "Hello" inside `file1.txt` using `grep`.
   - Count the number of lines in `file1.txt`.

---

## **Intermediate Tasks**
5. **User & Group Management**  
   - Create a new user named `student1` (use `sudo adduser student1`).  
   - Add `student1` to the `sudo` group.  
   - Create a new group called `dev_team`.  
   - Add `student1` to the `dev_team` group.  

6. **Process & Resource Management**  
   - List all running processes.  
   - Find the PID of a process (e.g., `firefox` or `chrome`).  
   - Kill a specific process using its PID.  
   - Check system memory usage.  

7. **Networking Tasks**  
   - Check the system's IP address.  
   - Ping a website like `google.com` and record the response time.  
   - Display all open network ports.  

8. **Archiving & Compression**  
   - Create a compressed archive of `my_project` using `tar`.  
   - Extract the archive to another location.  
   - Zip a file and unzip it to another directory.  

---

## **Advanced Tasks**
9. **Automation & Scripting**  
   - Write a script that prints "Hello, Linux!" and the current date.  
   - Modify the script to take a user’s name as input and greet them.  
   - Schedule the script to run every day at 10 AM using `cron`.  

10. **System Monitoring & Security**  
   - Check disk space usage.  
   - List all users logged into the system.  
   - Monitor system logs for login attempts.  

---

## **Bonus Challenge**
- Write a **script** that:
  - Creates a backup of a folder.
  - Appends the current date to the backup filename.
  - Deletes backups older than 7 days.
