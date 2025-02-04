## **1. DevOps & AWS Practical Scenarios**
**Apply Linux file management skills** in DevOps and cloud environments.

---

### **Scenario 1: Automated Log Rotation in a Linux Server**
**Problem:** Log files grow over time and take up space. You need to archive logs weekly and delete older backups.

**Solution:** Create a **log rotation script** that:
- Renames log files with timestamps.
- Moves them to an archive directory.
- Deletes logs older than 30 days.

**Script:**
```bash
#!/bin/bash

LOG_DIR="/var/log/myapp/"
ARCHIVE_DIR="/var/log/myapp/archive/"

# Create archive directory if not exists
mkdir -p "$ARCHIVE_DIR"

# Rename and move logs
for file in "$LOG_DIR"/*.log; do
    mv "$file" "$ARCHIVE_DIR/$(basename "$file")_$(date +%F).log"
done

# Delete logs older than 30 days
find "$ARCHIVE_DIR" -type f -name "*.log" -mtime +30 -exec rm {} \;

echo "Log rotation completed."
```

**Set up cron job:**
```bash
crontab -e
```
Add this line to run the script every Sunday at midnight:
```bash
0 0 * * 0 /path/to/script.sh
```

---

### **Scenario 2: Syncing Files to AWS S3**
**Problem:** DevOps teams often need to sync logs, backups, or static website files to AWS S3 for storage and disaster recovery.

**Solution:** Use `aws s3 sync` to copy files.

**Steps:**
1. Install AWS CLI:
   ```bash
   sudo apt install awscli
   ```
2. Configure AWS CLI:
   ```bash
   aws configure
   ```
3. Sync files:
   ```bash
   aws s3 sync /var/log/myapp/ s3://mybucket/logs/
   ```
4. Automate with a cron job:
   ```bash
   0 * * * * aws s3 sync /var/log/myapp/ s3://mybucket/logs/
   ```

---

### **Scenario 3: Automating EC2 Backups**
**Problem:** You need to back up important files from an **EC2 instance** to another location before a system update.

**Solution:** Create a script that:
- Copies important files to a backup directory.
- Syncs backups to another server or S3.

**Script:**
```bash
#!/bin/bash

BACKUP_DIR="/home/ubuntu/backups"
SOURCE_DIR="/var/www/html"
TIMESTAMP=$(date +%F_%H-%M-%S)

# Create a timestamped backup
mkdir -p "$BACKUP_DIR"
tar -czvf "$BACKUP_DIR/backup_$TIMESTAMP.tar.gz" "$SOURCE_DIR"

# Sync backup to S3
aws s3 cp "$BACKUP_DIR/backup_$TIMESTAMP.tar.gz" s3://mybackupbucket/

echo "Backup completed."
```

**Schedule the backup:**
```bash
0 2 * * * /path/to/backup_script.sh
```
(Runs every day at 2 AM)

---

### **Scenario 4: Terraform State Backup**
**Problem:** Terraform stores infrastructure state in a file called `terraform.tfstate`, which must be backed up securely.

**Solution:** Use `mv` and `cp` to create a backup before applying changes.

**Backup Terraform state:**
```bash
cp terraform.tfstate terraform.tfstate.backup_$(date +%F_%H-%M-%S)
```

**Automate the backup before Terraform apply:**
Modify `.bashrc` to add an alias:
```bash
alias tfapply='cp terraform.tfstate terraform.tfstate.backup_$(date +%F_%H-%M-%S) && terraform apply'
```
Now, running `tfapply` will **backup Terraform state and then apply changes**.

---

### **Scenario 5: Securely Copy Files Between Servers**
**Problem:** DevOps teams often copy configuration files or logs between Linux servers.

**Solution:** Use `scp` or `rsync` over SSH.

#### **Copy a file to another server using `scp`:**
```bash
scp file.txt user@remote-server:/home/user/
```

#### **Copy a directory using `rsync`:**
```bash
rsync -avz /var/log/myapp/ user@remote-server:/backup/logs/
```
- `-a` (archive mode, preserves attributes)
- `-v` (verbose output)
- `-z` (compress data)

---

### **Scenario 6: Deploying Code Updates**
**Problem:** You need to deploy a new version of your application from a local machine to a production server.

**Solution:** Use `scp` or `rsync` to copy updated files.

**Steps:**
1. Develop locally and test.
2. Copy files to the server.
   ```bash
   scp -r myapp/ user@server:/var/www/
   ```
3. Restart the service:
   ```bash
   ssh user@server "sudo systemctl restart apache2"
   ```

---

## **Bonus Activities for Students**
### **Activity 1: Automate User Home Directory Backups**
- Write a script that **copies all user home directories** to a backup location.
- Run it **daily using cron**.

**Example Script:**
```bash
#!/bin/bash

BACKUP_DIR="/backup/users/"
mkdir -p "$BACKUP_DIR"

for user in $(ls /home); do
    tar -czvf "$BACKUP_DIR/${user}_backup_$(date +%F).tar.gz" "/home/$user"
done
```

---

### **Activity 2: Securely Transfer Terraform State to an S3 Bucket**
- Write a script that **automatically backs up `terraform.tfstate` to an S3 bucket** before every Terraform apply.
- Modify the Terraform workflow to **always execute this script** before changes.

---

### **Activity 3: Automate S3 Sync for Website Deployment**
- Create a Bash script that **syncs a local website directory** to an **S3 bucket** for static hosting.

```bash
aws s3 sync /home/user/mywebsite/ s3://mywebsite-bucket/ --delete
```
- Schedule it with cron to **update the site automatically** every hour.

---

✅ **Understand Linux file operations in real-world cloud environments**  
✅ **Learn to automate backups, deployments, and infrastructure management**  
✅ **Enhance cybersecurity skills by securing Terraform states and user data**  
