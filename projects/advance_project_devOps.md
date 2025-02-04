## **1. AWS Lambda: Serverless Image Processing with S3**
### **Project: Automatically Resize Images with AWS Lambda**
🔹 **Objective:** Use AWS Lambda to **automatically resize images** when uploaded to an **S3 bucket**.

#### **Step 1: Create an S3 Bucket**
```bash
aws s3 mb s3://image-upload-bucket-123
aws s3 mb s3://image-resized-bucket-123
```

#### **Step 2: Create a Lambda Function (Python)**
```python
import boto3
from PIL import Image
import io

s3 = boto3.client('s3')

def lambda_handler(event, context):
    for record in event['Records']:
        bucket_name = record['s3']['bucket']['name']
        file_name = record['s3']['object']['key']

        # Download the image
        file_obj = s3.get_object(Bucket=bucket_name, Key=file_name)
        image = Image.open(io.BytesIO(file_obj['Body'].read()))

        # Resize and save
        image = image.resize((300, 300))
        out_buffer = io.BytesIO()
        image.save(out_buffer, format="JPEG")

        # Upload resized image
        s3.put_object(Bucket='image-resized-bucket-123', Key=file_name, Body=out_buffer.getvalue())

    return {"statusCode": 200, "body": "Image resized successfully"}
```

#### **Step 3: Deploy the Lambda Function**
1. **Create a zip package** for the Lambda function:
   ```bash
   zip function.zip lambda_function.py
   ```

2. **Deploy it in AWS:**
   ```bash
   aws lambda create-function --function-name ResizeImage \
   --runtime python3.8 --role arn:aws:iam::123456789012:role/LambdaS3Role \
   --handler lambda_function.lambda_handler --zip-file fileb://function.zip
   ```

✔️ **Now, every time an image is uploaded, it will be resized automatically.**  

---

## **2. AWS ECS: Deploy a Dockerized Web App**
### **Project: Deploy a Scalable Web App Using ECS and Fargate**
🔹 **Objective:** Deploy a containerized web application on **AWS ECS (Elastic Container Service)** using **Fargate**.

#### **Step 1: Create a Docker Image**
```dockerfile
FROM nginx:latest
COPY index.html /usr/share/nginx/html/
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
```
1. **Build and push to AWS ECR:**
   ```bash
   aws ecr create-repository --repository-name my-web-app
   docker build -t my-web-app .
   docker tag my-web-app:latest 123456789012.dkr.ecr.us-east-1.amazonaws.com/my-web-app:latest
   aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin 123456789012.dkr.ecr.us-east-1.amazonaws.com
   docker push 123456789012.dkr.ecr.us-east-1.amazonaws.com/my-web-app:latest
   ```

#### **Step 2: Create an ECS Cluster**
```bash
aws ecs create-cluster --cluster-name web-app-cluster
```

#### **Step 3: Define ECS Task & Service**
```json
{
  "containerDefinitions": [
    {
      "name": "web-container",
      "image": "123456789012.dkr.ecr.us-east-1.amazonaws.com/my-web-app:latest",
      "memory": 512,
      "cpu": 256,
      "essential": true,
      "portMappings": [
        {
          "containerPort": 80,
          "hostPort": 80
        }
      ]
    }
  ]
}
```

#### **Step 4: Deploy on ECS**
```bash
aws ecs create-service --cluster web-app-cluster --service-name web-app-service \
--task-definition web-app-task --launch-type FARGATE \
--desired-count 2
```

✔️ **Now, your web app is running in a scalable ECS environment!**

---

## **3. Kubernetes: Deploy a Flask App on AWS EKS**
### **Project: Containerized Flask App on AWS EKS**
🔹 **Objective:** Deploy a **Flask web app** on **Amazon EKS (Elastic Kubernetes Service)**.

#### **Step 1: Install and Configure kubectl & eksctl**
```bash
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
chmod +x kubectl && sudo mv kubectl /usr/local/bin/
```
```bash
curl --silent --location "https://github.com/weaveworks/eksctl/releases/latest/download/eksctl_Linux_amd64.tar.gz" | tar xz -C /tmp
sudo mv /tmp/eksctl /usr/local/bin/
```

#### **Step 2: Create an EKS Cluster**
```bash
eksctl create cluster --name flask-cluster --region us-east-1 --nodegroup-name workers \
--node-type t3.medium --nodes 2
```

#### **Step 3: Write a Flask App (`app.py`)**
```python
from flask import Flask
app = Flask(__name__)

@app.route('/')
def home():
    return "Hello from Flask in Kubernetes!"

if __name__ == '__main__':
    app.run(host='0.0.0.0', port=5000)
```

#### **Step 4: Create a Docker Image & Push to ECR**
```bash
docker build -t flask-app .
docker tag flask-app:latest 123456789012.dkr.ecr.us-east-1.amazonaws.com/flask-app:latest
docker push 123456789012.dkr.ecr.us-east-1.amazonaws.com/flask-app:latest
```

#### **Step 5: Deploy to EKS with YAML Files**
Create `deployment.yaml`:
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: flask-app
spec:
  replicas: 2
  selector:
    matchLabels:
      app: flask
  template:
    metadata:
      labels:
        app: flask
    spec:
      containers:
      - name: flask
        image: 123456789012.dkr.ecr.us-east-1.amazonaws.com/flask-app:latest
        ports:
        - containerPort: 5000
```

Create `service.yaml`:
```yaml
apiVersion: v1
kind: Service
metadata:
  name: flask-service
spec:
  selector:
    app: flask
  ports:
  - protocol: TCP
    port: 80
    targetPort: 5000
  type: LoadBalancer
```

#### **Step 6: Deploy to Kubernetes**
```bash
kubectl apply -f deployment.yaml
kubectl apply -f service.yaml
```

✔️ **Now, the Flask app is running on EKS with auto-scaling!**

---

## **Summary of Projects**
| **Project** | **AWS Service** | **Key Skills** |
|------------|---------------|------------|
| Serverless Image Processing | Lambda, S3 | Python, Automation |
| Deploy Web App with ECS | ECS, Fargate | Docker, AWS ECR |
| Kubernetes Cluster for Flask | EKS, Kubernetes | k8s, Container Orchestration |
