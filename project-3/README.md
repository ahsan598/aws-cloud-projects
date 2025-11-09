# Lift & Shift Migration of Web Application to AWS Cloud


## 🎯 Project Overview
**Goal:**
Lift & shift a legacy monolithic web application (Tomcat) from on-prem to AWS, using managed AWS services for scalability, High Availability, and cost efficiency — minimal changes to app architecture.


### AWS Services Used & Architecture Diagram:

| Layer               |                         Key Services & Components                       |
| ------------------- | ----------------------------------------------------------------------- |
| **Networking**          | VPC, Public & Private Subnets, IGW, NAT, Route Tables, SG & NACLs       |
| **Compute**             | EC2 Instances, Auto Scaling Group                                       |
| **Load Balancing**      | Application Load Balancer (ALB), ACM for SSL                            |
| **Database**            | Amazon RDS (MySQL/PostgreSQL, Multi-AZ)                                 |
| **Messaging**           | RabbitMQ (on EC2, port 5672)                                            |
| **Caching**             | Memcached (on EC2, port 11211)                                          |
| **Storage**             | Amazon S3 (static files, backups)                                       |
| **DNS**                 | Route 53 (public DNS + private hosted zone), Hostinger (external zone)  |
| **Security**            | IAM Roles/Policies following least-privilege principle                  |
| **Observability**       | CloudWatch, S3 for logs/billing                                         |


![Lift-Shift-Architecture](/assets/images/project-3.png)


 
## 🛠️ Implementation Steps (Console-based)

**1. Networking:**
- Create VPC + subnets for separation of web app, DB, and backend.
- Configure route tables, attach IGW/NAT GW, define security rules.

**2 IAM & Security:**
- Create IAM roles for EC2, RDS (S3, logging, access control).
- Apply least-privilege to all compute resources.

**3. EC2 Setup:**
- Launch Amazon Linux EC2, install Tomcat (yum install tomcat).
- Enable/secure EC2 instances with proper security groups.

**4. ALB & ACM (SSL):**
- Deploy Application Load Balancer, attach ACM cert for HTTPS.
- Register EC2s with ALB target group and enable health checks.

**5. Auto Scaling:**
- Create launch template and auto scaling group for Tomcat EC2s.
- Link with ALB for flexible, failover scaling.

**6. RDS (DB):**
- Deploy RDS (MySQL or PostgreSQL) in private subnets, enable Multi-AZ.
- Set DB SG to allow access only from app EC2s.

**7. Messaging & Caching:**
- Launch RabbitMQ and Memcached on EC2s, allow access only within VPC.

**8. S3 Storage:**
- Set up S3 for app static assets/backups; grant access via IAM roles.

**9. Route 53 & DNS:**
- Configure Route 53 private hosted zone for internal resolution.
- Optionally use public zone (or Hostinger) for external routing.



## 🔍 Validation & Testing
- Access app via ALB DNS (or domain)
- Confirm SSL, scaling, and DB connection
- Test queue/caching functionality (RabbitMQ, Memcached)
- SSH/Bastion access (if required) for private instances
- Monitor resources via CloudWatch



## 💡 Notes & Best Practices
- All EC2, RDS, and app resources locked to private subnets (security)
- IAM policies are least-privilege and tightly scoped
- Messaging/caching still on EC2 for simulating on-prem patterns (ElastiCache/MQ are the managed alternatives)
- ACM ensures traffic is secured via SSL at ALB
- S3 for all static assets, logs, and backups



## ⚠️ Important
- **Cost Optimization:** Use auto scaling and disable resources when not in use.
- **Monitoring:** Enable CloudWatch for health and billing.
- **Security:** Regularly review IAM roles, security group rules, and update as per best practices.