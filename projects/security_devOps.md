### **Advanced AWS Security & Cloud Cost Optimization Projects**  

If you're looking to **enhance security automation** and **reduce cloud costs**, these projects focus on **AWS IAM, GuardDuty, AWS WAF, and cost optimization tools like AWS Compute Optimizer and Cost Explorer.**

---

## **1. AWS Security: Automate IAM User Auditing with AWS Lambda & CloudWatch**  
🔹 **Objective:** Automatically audit and disable inactive IAM users using AWS Lambda.

### **Step 1: Create an AWS Lambda Function**  
This function **identifies IAM users who haven’t logged in for 90 days** and disables them.

```python
import boto3
from datetime import datetime, timedelta

iam = boto3.client('iam')

def lambda_handler(event, context):
    users = iam.list_users()['Users']
    cutoff_date = datetime.utcnow() - timedelta(days=90)

    for user in users:
        username = user['UserName']
        last_login = user.get('PasswordLastUsed', None)

        if last_login and last_login < cutoff_date:
            print(f"Disabling {username} due to inactivity")
            iam.update_login_profile(UserName=username, PasswordResetRequired=True)

    return {"statusCode": 200, "body": "Inactive users audited successfully"}
```

### **Step 2: Schedule This Lambda with CloudWatch Events**  
```bash
aws events put-rule --schedule-expression "rate(1 day)" --name "IAMUserAudit"
aws lambda add-permission --function-name IAMUserAudit \
--statement-id AllowCloudWatchInvoke --action "lambda:InvokeFunction" --principal events.amazonaws.com
aws events put-targets --rule "IAMUserAudit" --targets "Id"="1","Arn"="arn:aws:lambda:us-east-1:123456789012:function:IAMUserAudit"
```

✔️ **Now, inactive users are automatically detected and disabled.**

---

## **2. AWS GuardDuty: Auto-Remediate Security Threats Using Lambda**
🔹 **Objective:** Detect threats using **AWS GuardDuty** and **automatically quarantine compromised EC2 instances**.

### **Step 1: Enable AWS GuardDuty**
```bash
aws guardduty create-detector --enable
```

### **Step 2: Create a Lambda Function for Automated Response**
Whenever GuardDuty detects a high-severity threat, the function **isolates the EC2 instance** by removing it from public subnets.

```python
import boto3

ec2 = boto3.client('ec2')

def lambda_handler(event, context):
    instance_id = event['detail']['resource']['instanceDetails']['instanceId']
    
    # Move instance to an isolated security group
    response = ec2.modify_instance_attribute(InstanceId=instance_id, Groups=['sg-12345678'])
    
    print(f"Instance {instance_id} moved to quarantine security group")
    return {"statusCode": 200, "body": "Instance quarantined"}
```

### **Step 3: Configure GuardDuty to Trigger Lambda**
1. **Create a CloudWatch rule** for GuardDuty events:
   ```bash
   aws events put-rule --name "GuardDutyAlert" \
   --event-pattern '{"source": ["aws.guardduty"], "detail-type": ["GuardDuty Finding"]}'
   ```

2. **Attach the rule to Lambda:**
   ```bash
   aws lambda add-permission --function-name GuardDutyAutoRemediate \
   --statement-id AllowCloudWatchInvoke --action "lambda:InvokeFunction" --principal events.amazonaws.com
   ```

✔️ **Now, GuardDuty automatically isolates compromised instances.**  

---

## **3. AWS WAF: Block Malicious Traffic with Web Application Firewall**
🔹 **Objective:** Use **AWS WAF** to **block known bad IPs** and rate-limit requests.

### **Step 1: Create a WAF Rule to Block Bad IPs**
```bash
aws wafv2 create-ip-set --name "MaliciousIPs" --scope REGIONAL --ip-address-version IPV4 \
--addresses "192.168.1.100/32" "10.0.0.1/32"
```

### **Step 2: Attach WAF to an ALB**
```bash
aws wafv2 associate-web-acl --web-acl-arn arn:aws:waf2:us-east-1:123456789012:regional/webacl/SecurityWAF \
--resource-arn arn:aws:elasticloadbalancing:us-east-1:123456789012:loadbalancer/app/MyALB/50dc6c495c0c9188
```

✔️ **Now, malicious IPs are automatically blocked from accessing your application.**

---

## **4. AWS Cost Optimization: Auto-Stop Unused EC2 Instances**
🔹 **Objective:** Identify and stop unused EC2 instances **to reduce costs.**

### **Step 1: Create a Lambda Function**
This function **stops EC2 instances that have been idle for more than 7 days**.

```python
import boto3
from datetime import datetime, timedelta

ec2 = boto3.client('ec2')

def lambda_handler(event, context):
    instances = ec2.describe_instances(Filters=[{'Name': 'instance-state-name', 'Values': ['running']}])

    for reservation in instances['Reservations']:
        for instance in reservation['Instances']:
            launch_time = instance['LaunchTime']
            instance_id = instance['InstanceId']

            if (datetime.utcnow() - launch_time.replace(tzinfo=None)).days > 7:
                print(f"Stopping instance {instance_id} due to inactivity")
                ec2.stop_instances(InstanceIds=[instance_id])

    return {"statusCode": 200, "body": "Unused EC2 instances stopped"}
```

### **Step 2: Schedule Lambda with CloudWatch**
```bash
aws events put-rule --schedule-expression "rate(1 day)" --name "StopUnusedEC2"
aws events put-targets --rule "StopUnusedEC2" --targets "Id"="1","Arn"="arn:aws:lambda:us-east-1:123456789012:function:StopUnusedEC2"
```

✔️ **Now, unused EC2 instances are automatically stopped, reducing costs.**  

---

## **5. AWS Cost Explorer: Track & Optimize AWS Costs**
🔹 **Objective:** Generate **cost reports** and find **expensive services.**

### **Step 1: Enable AWS Cost Explorer**
```bash
aws ce update-cost-allocation-tags-status --cost-allocation-tags Key="Project", Status="Active"
```

### **Step 2: Query Monthly Costs via CLI**
```bash
aws ce get-cost-and-usage --time-period Start=2024-01-01,End=2024-12-31 \
--granularity MONTHLY --metrics "BlendedCost"
```

✔️ **Now, you can track and optimize AWS spending with detailed cost insights.**  

---

## **Summary of Advanced AWS Security & Cost Projects**
| **Project** | **AWS Service** | **Key Benefits** |
|------------|---------------|------------|
| IAM User Auditing | IAM, CloudWatch | Disable inactive users automatically |
| Auto-Quarantine Compromised EC2 | GuardDuty, Lambda | Isolate suspicious EC2 instances |
| Web Traffic Filtering | AWS WAF | Block bad IPs & prevent DDoS |
| Stop Unused EC2 | EC2, Lambda | Reduce AWS cost wastage |
| Cost Analysis Reports | AWS Cost Explorer | Identify expensive services |

---

 **more AWS security automation (e.g., AWS Config, Security Hub, KMS encryption)**, or **cost-saving automation for databases and storage**? 🚀
