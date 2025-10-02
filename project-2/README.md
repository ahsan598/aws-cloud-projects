# <img src="https://github.com/devicons/devicon/blob/master/icons/amazonwebservices/amazonwebservices-original-wordmark.svg" alt="AWS" width="50"/> Deploying a WordPress Website on AWS


This project demonstrates how to deploy a **highly available**, **scalable**, and **secure** WordPress website on AWS using core cloud services and best practices.

The architecture spans across multiple availability zones and includes components like **ALB**, **EC2**, **RDS**, **S3**, **Route 53**, **IAM**, and **CloudWatch**.

---

### ☁️ Resources Provisioned
The following components are created in this setup:

**1. Networking Layer**  
- VPC with **Public** and **Private Subnets** across **2 AZs**  
- **Internet Gateway** for external connectivity  
- **NAT Gateway** for private subnet internet access  
- **Security Groups** for EC2 and RDS  

**2. Access & Security**  
- **Bastion Host** in Public Subnet for SSH access  
- **IAM Role** for EC2 → S3 access  

**3. Compute & Database**  
- **EC2 Instances** in Private Subnets (WordPress)  
- **Amazon RDS (MySQL)** in Private Subnets (Multi-AZ)  

**4. Scalability & Load Balancing**  
- **Auto Scaling Group (ASG)** for EC2  
- **Application Load Balancer (ALB)** in Public Subnets  

**5. Storage & DNS**  
- **Amazon S3** for WordPress assets  
- **Route 53** for domain and DNS resolution  

**6. Monitoring**  
- **CloudWatch** for metrics, logs, and alarms


### 🔧 Project Architecture:

![Architecture](https://github.com/ahsan598/devops-projects-hands-on/blob/main/project-2-aws-wordpress-setup/img/aws-wordpress-website-diagram.svg)

---

### ⚙️ Implementation (Using AWS Console)

Here’s an overview of how the WordPress hosting environment was configured:

**1. VPC & Subnet Setup**
- Create a custom **VPC** with **2 public** and **2 private** subnets across **2 AZs**
- Attach an **Internet Gateway (IGW)** to the VPC
- Create and associate **route tables** for public/private subnets

**2. NAT Gateway (for Private Subnet Internet Access)**
- Launch NAT Gateway in each public subnet
- Allocate Elastic IPs
- Add routes from private subnets to NAT Gateway

**3. Bastion Host**
- Launch **EC2 instance** in public subnet (Amazon Linux)
- Assign Elastic IP and Security Group with SSH access
- Use this host to SSH into EC2s in private subnet

**4. EC2 Instances for WordPress**
- Launch **EC2 instances** in private subnets (via Bastion Host)
- Attach **IAM Role** allowing access to S3
- Install **Apache, PHP, WordPress**, and configure settings

**5. S3 Bucket for Website Assets**
- Upload WordPress themes, media, plugins etc. to an S3 bucket
- Ensure correct bucket policy and access via EC2 IAM role

**6. RDS – MySQL Database**
- Create **Amazon RDS (MySQL)** in private subnet
- Enable Multi-AZ for high availability
- Connect from EC2 via internal endpoint

**7. Application Load Balancer (ALB)**
- Create **ALB** in public subnets
- Target group pointing to private EC2 instances
- Configure listener on port 80 (or HTTPS if SSL added)

**8. Auto Scaling Group (ASG)**
- Define launch template with EC2 setup
- Attach ASG to target group
- Set scaling policies (CPU, health checks)

**9. Route 53 & Domain Setup**
- Register domain or create a hosted zone
- Point domain to ALB DNS name via A record (Alias)

**10. Monitoring with CloudWatch**
- Set up CloudWatch alarms for EC2, ALB, RDS metrics
- Collect logs from Apache or PHP using CloudWatch Agent


### 🔍 Testing the Deployment

- Visit domain via browser → check WordPress home page
- Test login, uploads (via S3), DB operations (via RDS)
- Scale EC2 instances and test auto-scaling behavior
- Terminate an EC2 instance and observe ASG response

---

### 📚 Learnings Outcome

- Real-world VPC architecture for secure multi-tier web application
- Use of ALB + ASG for load balancing and scalability
- Integration of S3, RDS, Route 53, IAM Roles, and CloudWatch
- Deep understanding of manual AWS provisioning (a step before automation)


### ⚠️ Important Note
- NAT Gateway is a **billable per-hour** service — keep it disabled or delete when not in use.
- Delete **all resources** when not testing to avoid unexpected AWS charges.

---

> 💡 _"Before you automate with Terraform or CloudFormation, understand what you're building manually."_
