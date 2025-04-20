# 📸 TagMe – AI-Powered Photo Management App
<img width="1236" alt="cloud" src="https://github.com/user-attachments/assets/3557fd88-fdd8-4f51-97cc-86bbe7a400fe" />

TagMe is a cloud-based photo management web application that uses AWS Rekognition to automatically tag images with descriptive labels. Users can upload, store, and search their image libraries using AI-generated tags.

---

## 🧩 Table of Contents
- [Demo](#demo)
- [Features](#Features)
- [Architecture](#Architecture)
- [AWS Services Used](#Aws-Services-Used)
- [Tech Stack](#Tech-Stack)
- [Deployment](#Deployment)
- [Security](#Security)
- [Cost](#Cost)

---

## DEMO

https://github.com/user-attachments/assets/786618ba-b1ee-4cec-a370-87de97bb95ef

---

## Features

- Upload and tag images using AI (AWS Rekognition)
- Search photos by tags
- User authentication with secure sessions
- Responsive and minimal UI (mobile & desktop)
- Real-time image processing pipeline
- Handles up to 10 concurrent users

---

## Architecture

1. **User uploads photo via EC2-hosted frontend**
2. **Image is stored in S3**
3. **S3 triggers a message to SQS**
4. **SQS invokes a Lambda function**
5. **Lambda processes the image with Rekognition**
6. **Results (tags) are saved to RDS**
7. **Frontend queries RDS to display tagged photos**

### Cloud Setup
- All resources are contained in a VPC 
- EC2 instance in public subnet 
- Lambda + RDS in private subnet
- NAT Gateway enables private subnet access to S3/Rekognition

---

## AWS Services Used

| Category                 | Services Used                                |
|--------------------------|-----------------------------------------------|
| Compute                  | Amazon EC2, AWS Lambda                        |
| Storage                  | Amazon S3                                     |
| Networking               | Amazon VPC, NAT Gateway, Security Groups      |
| Database                 | Amazon RDS (MySQL)                            |
| Application Integration  | Amazon SQS                                    |
| Monitoring & Logging     | Amazon CloudWatch                             |
| Image Analysis (AI)      | Amazon Rekognition                            |

---

## Tech Stack

- **Frontend:** HTML, CSS, JavaScript, Bootstrap
- **Web Server:** Node.js (Express)
- **Backend (Lambda):** Node.js with Rekognition API
- **Database:** MySQL (via Amazon RDS)


![Untitled](https://github.com/user-attachments/assets/03c54c3a-8f50-4ad3-acea-7315cb0a13c7)

- **Deployment:** AWS CloudFormation (YAML IaC)
- **Scripting:** Bash + Curl for EC2 provisioning

---

## Deployment

This application uses **Infrastructure as Code (IaC)** with AWS CloudFormation for fully automated provisioning of all services and dependencies. Launching the stack builds the full cloud architecture in a single click.

---

## Security

- VPC with isolated public/private subnets
- EC2 security groups restrict access to ports 80 (HTTP) and 22 (SSH)
- RDS access limited to port 3306 within VPC
- S3 access restricted to authenticated users
- Session data securely stored in RDS
- Passwords hashed with bcrypt
- MySQL prepared statements protect against SQL injection
- Secrets stored in environment variables

**Note:** For educational/development purposes only. Future versions will include Cognito, HTTPS, encryption at rest, and IAM roles for finer-grained access control.

---

## Cost Evaluation

### Estimated Monthly Cost (Post-Free Tier):

| Resource       | Cost Estimate     |
|----------------|------------------|
| EC2 (t2.micro) | ~$8.47           |
| Elastic IP     | ~$3.65           |
| NAT Gateway    | ~$32.85          |
| **Total**      | **~$45/month**   |

### Notes:
- Free Tier covers 12 months of EC2, RDS, and basic S3 usage
- SQS is free for the first 1M requests/month
- S3 charges ~$0.023/GB after 5GB free
- NAT data transfer charges apply ($0.045/GB)

Cost can be reduced using S3 archival tiers, Aurora Serverless, or private VPC endpoints.

