## **1. AWS & Terraform: Automating Infrastructure Deployment**
### **Project: Automate EC2 & S3 Setup with Terraform**
🔹 **Objective:** Use Terraform to provision an **EC2 instance**, create an **S3 bucket**, and configure a basic web server.

#### **Step 1: Install Terraform & AWS CLI**
```bash
sudo apt install awscli -y
curl -fsSL https://apt.releases.hashicorp.com/gpg | sudo gpg --dearmor -o /usr/share/keyrings/hashicorp-archive-keyring.gpg
echo "deb [signed-by=/usr/share/keyrings/hashicorp-archive-keyring.gpg] https://apt.releases.hashicorp.com $(lsb_release -cs) main" | sudo tee /etc/apt/sources.list.d/hashicorp.list
sudo apt update && sudo apt install terraform
```

#### **Step 2: Write Terraform Code (`main.tf`)**
```hcl
provider "aws" {
  region = "us-east-1"
}

resource "aws_instance" "web_server" {
  ami           = "ami-0c55b159cbfafe1f0"  # Amazon Linux 2 AMI
  instance_type = "t2.micro"
  tags = {
    Name = "Terraform-WebServer"
  }
}

resource "aws_s3_bucket" "my_bucket" {
  bucket = "my-terraform-bucket-12345"
  acl    = "private"
}

output "instance_ip" {
  value = aws_instance.web_server.public_ip
}
```

#### **Step 3: Deploy Infrastructure**
```bash
terraform init
terraform apply -auto-approve
```

✔️ This will create an EC2 instance and an S3 bucket.  

---

## **2. DevOps: CI/CD Pipeline with Jenkins & GitHub**
### **Project: Automate Deployments with Jenkins**
🔹 **Objective:** Set up a **Jenkins pipeline** to automatically deploy code from GitHub to an AWS EC2 instance.

#### **Step 1: Install Jenkins on an EC2 Server**
```bash
sudo yum update -y
sudo amazon-linux-extras install epel -y
sudo yum install java-11-openjdk-devel -y
wget -O /etc/yum.repos.d/jenkins.repo https://pkg.jenkins.io/redhat-stable/jenkins.repo
sudo rpm --import https://pkg.jenkins.io/redhat-stable/jenkins.io-2023.key
sudo yum install jenkins -y
sudo systemctl enable jenkins
sudo systemctl start jenkins
```

#### **Step 2: Create a Jenkins Pipeline**
1. **Go to Jenkins Dashboard** → New Item → Pipeline.
2. **Write the Pipeline Script:**
```groovy
pipeline {
    agent any
    stages {
        stage('Clone Repo') {
            steps {
                git 'https://github.com/your-repo/app.git'
            }
        }
        stage('Build') {
            steps {
                sh 'mvn clean package'
            }
        }
        stage('Deploy') {
            steps {
                sshagent(['ec2-ssh-key']) {
                    sh 'scp target/app.war ec2-user@my-server:/var/www/html/'
                }
            }
        }
    }
}
```
3. **Trigger on GitHub Push:**  
   - Go to GitHub Repo → Settings → Webhooks → Add Jenkins Webhook.

✔️ Now, any code push will trigger **automatic deployment to the EC2 server**.

---

## **3. AWS: Monitor EC2 Instance with CloudWatch & Alerts**
### **Project: Set Up CPU Usage Alerts for EC2**
🔹 **Objective:** Use **AWS CloudWatch** to monitor an EC2 instance and send alerts when CPU usage exceeds 80%.

#### **Step 1: Install CloudWatch Agent on EC2**
```bash
sudo yum install amazon-cloudwatch-agent -y
```

#### **Step 2: Create a CloudWatch Alarm**
```bash
aws cloudwatch put-metric-alarm --alarm-name "HighCPUUsage" --metric-name CPUUtilization \
  --namespace AWS/EC2 --statistic Average --period 60 --threshold 80 \
  --comparison-operator GreaterThanThreshold --dimensions Name=InstanceId,Value=i-1234567890abcdef \
  --evaluation-periods 1 --alarm-actions arn:aws:sns:us-east-1:1234567890:MyTopic
```

✔️ **Now, if CPU usage exceeds 80%, an SNS alert will be sent to administrators.**  

---

## **4. Security: Automate SSH Key Rotation for EC2 Instances**
### **Project: Enforce Secure SSH Key Management**
🔹 **Objective:** Rotate SSH keys every **7 days** to enhance security.

#### **Step 1: Generate a New SSH Key**
```bash
ssh-keygen -t rsa -b 4096 -f ~/.ssh/new_key -N ""
```

#### **Step 2: Replace Old Key on Remote Servers**
```bash
for server in $(cat servers.txt); do
    ssh-copy-id -i ~/.ssh/new_key.pub ec2-user@$server
done
```

✔️ **Automate this with cron to rotate keys weekly.**

---

## **5. Linux & AWS: Automate Backup & Restore**
### **Project: Daily Backup of Important Files to AWS S3**
🔹 **Objective:** Automate backups of important Linux files to an AWS S3 bucket.

#### **Step 1: Install AWS CLI**
```bash
sudo apt install awscli -y
aws configure
```

#### **Step 2: Write a Backup Script**
```bash
#!/bin/bash
BACKUP_DIR="/var/backups"
S3_BUCKET="s3://my-backup-bucket"
TIMESTAMP=$(date +%F_%H-%M-%S)

# Create Backup
tar -czvf "$BACKUP_DIR/backup_$TIMESTAMP.tar.gz" /etc /home

# Upload to S3
aws s3 cp "$BACKUP_DIR/backup_$TIMESTAMP.tar.gz" "$S3_BUCKET/"

echo "Backup completed at $TIMESTAMP"
```

#### **Step 3: Schedule a Daily Backup**
```bash
crontab -e
```
Add:
```bash
0 3 * * * /path/to/backup_script.sh
```

✔️ **Now, backups will be uploaded to S3 every night at 3 AM.**

---

## **6. Terraform: Manage AWS IAM Users & Roles**
### **Project: Automate IAM User Creation**
🔹 **Objective:** Use Terraform to create IAM users with specific permissions.

#### **Terraform Code:**
```hcl
provider "aws" {
  region = "us-east-1"
}

resource "aws_iam_user" "developer" {
  name = "dev_user"
}

resource "aws_iam_policy_attachment" "attach_policy" {
  name       = "dev_policy_attachment"
  users      = [aws_iam_user.developer.name]
  policy_arn = "arn:aws:iam::aws:policy/AmazonS3FullAccess"
}

output "user_arn" {
  value = aws_iam_user.developer.arn
}
```

#### **Deploy with Terraform**
```bash
terraform init
terraform apply -auto-approve
```

✔️ **Now, the IAM user `dev_user` is created with S3 access.**

---

## **Summary of Skills Covered**
| **Project** | **Skill** |
|------------|----------|
| Deploy EC2 & S3 with Terraform | AWS, Terraform |
| Jenkins CI/CD Deployment | DevOps, Automation |
| CloudWatch Monitoring & Alerts | AWS, Monitoring |
| SSH Key Rotation Automation | Security, Linux |
| S3 Backup & Restore | AWS, Linux Admin |
| IAM User Management with Terraform | AWS Security, Terraform |
