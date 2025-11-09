# Scalable WordPress Deployment on AWS


## 🎯 Project Overview
This project demonstrates the deployment of a highly available, scalable, and secure WordPress website on AWS, leveraging core cloud services and AWS best practices.

### Core Architecture
A real-world AWS stack for WordPress using:
- **Multi-AZ** custom VPC (2 public + 2 private subnets)
- **Public:** Bastion host, NAT Gateway, ALB
- **Private:** EC2 (WordPress) + RDS (MySQL Multi-AZ)
- **S3** for WordPress assets
- **Route 53** for DNS, CloudWatch for monitoring
- **IAM role** for secure S3 access from EC2


### Diagram Overview:
![AWS WordPress Architecture](/assets/images/project-2.png)

| Layer               | Description                                           |
| ------------------- | ----------------------------------------------------- |
| **Networking**      | VPC, Subnets, Route Tables, IGW, NAT                  |
| **Access/Security** | Bastion Host (SSH into private EC2), IAM Role         |
| **Compute**         | EC2 (WordPress Linux servers, in autoscaling group)   |
| **Database**        | RDS MySQL Multi-AZ (Master + Standby, private subnet) |
| **Scalability**     | ALB + Auto Scaling Group, multi-AZ load balancing     |
| **Storage**         | S3 bucket for assets (themes, media, plugins)         |
| **DNS**             | Route 53 pointed to ALB DNS                           |
| **Monitoring**      | CloudWatch alarms, logs, metrics                      |



## 🚀 Implementation Steps

**1. VPC & Subnets:**
- Create custom VPC with 2 public and 2 private subnets across 2 AZs.

**2. Internet, NAT & Bastion:**
- Attach IGW, launch NAT gateways (one per AZ), Bastion host in public subnet for SSH.

**3. EC2 + Autoscaling Group:**
- Launch WordPress EC2 instances (Amazon Linux) in private subnets.
- Configure IAM role for S3 access.
- Set up autoscaling group targeting private EC2s behind ALB.

**4. RDS MySQL:**
- Create Multi-AZ RDS in private subnet.
- Security group allow connections only from WordPress EC2.

**5. S3 Bucket:**
- Store WordPress media/plugins, enable permissions for EC2 IAM role.

**6. ALB (Application Load Balancer):**
- Create in public subnets, forward traffic to EC2s.

**7. Route 53:**
- Register domain/setup hosted zone.
- Create A record (Alias) pointing to ALB DNS.

**8. CloudWatch:**
- Enable metrics/logs for EC2, RDS, ALB.
- Set alerts for CPU, unhealthy hosts.


## 🧪 Validation & Testing
- Browse to domain: verify WordPress home page & login
- Confirm uploads, DB interaction
- Terminate EC2, watch autoscaling/self-heal
- Monitor metrics/alerts in CloudWatch


## 💡 Learnings
- Manual multi-AZ VPC setup, Subnet isolation
- Secure bastion host access pattern
- Load-balanced, auto-scaled WordPress architecture
- S3 integration for offloaded assets
- IAM roles for controlled access
- Internal-to-private RDS database (no public endpoints)
- CloudWatch for real observability



## ⚠️ Cost Optimization Note
- Disable/Delete NAT Gateway, RDS, and EC2s when not in use (these incur hourly charges).
- Remove all stack resources after testing/dev to avoid unexpected bills.
