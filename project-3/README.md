# Lift & Shift Web App on AWS Cloud

### 🎯 Purpose

The goal of this Lift & Shift project is to migrate an existing on-premises web application to AWS cloud with minimal changes to the application architecture. This approach provides:
- **Scalability** – Elastic compute resources
- **Cost Optimization** – Pay-as-you-go pricing model
- **Global Reach** – Multi-region infrastructure
- **High Availability** – Auto-scaling and failover mechanisms
- **Security & Compliance** – Managed identity and access controls
- **Faster Time to Market** – Use of AWS-managed services


### 📚 AWS Resources Used:

1. **Virtual Private Cloud (VPC)**:
- Custom **VPC** with CIDR block `10.0.0.0/16`
- **Subnets**: Public and Private in multiple AZs
- **Routing**: Route Tables, NAT Gateway, Internet Gateway
- **Security**: NACLs & Security Groups

2. **Identity and Access Management (IAM)**:
- **Roles & Policies**: For EC2, RDS, and other services
- **Principle**: Least privilege access

3. **EC2 with Tomcat**:
- Deployed app servers using Amazon Linux + Tomcat
- Web servers in Public Subnet, Backend services in Private

4. **Amazon S3**:
- Storage for static files and backups
- Enabled encryption and access policies

5. **Application Load Balancer (ALB)**:
- Distributes traffic across EC2 instances
- Path-based routing and health checks

6. **Auto Scaling**:
- Automatically adjusts EC2 instances based on load
- Integrated with ALB health checks

7. **Amazon RDS**:
- Managed MySQL/PostgreSQL database
- Private Subnet deployment with Multi-AZ setup

8. **RabbitMQ**:
- Installed on EC2 for asynchronous message handling
- Port `5672` allowed via security groups

9. **Memcached**:
- In-memory cache deployed on EC2
- Port `11211` allowed from app servers

10. **Route 53 & DNS Zones**:
**Definition:** An in-memory key-value store used to speed up dynamic web applications by reducing database load.
**Purpose:** Caches frequently requested data, such as API responses or rendered pages, to improve performance.

11. **Route 53 & DNS Zones**:
- **Private Hosted Zone**: For internal resolution
- **Public DNS (optional)**: For internet-facing domains


### 🗺️ Architecture Overview

![Project Diagram](https://github.com/ahsan598/devops-projects-hands-on/blob/main/project-5-aws-liftshift-webapp/aws-liftshift-webapp.png)

> Deployed architecture includes VPC, ALB, EC2 (Tomcat), RDS, RabbitMQ, Memcached, Auto Scaling, and Route 53.

---

### ⚙️ Implementation Steps

1. **VPC Setup**
```text
- Create VPC (CIDR: 10.0.0.0/16)
- Add Public & Private Subnets
- Configure Route Tables
- Attach Internet Gateway & NAT Gateway
- Create Security Groups
```

2. **IAM Setup**
```text
- Create IAM roles for EC2, RDS, etc.
- Attach policies like AmazonS3ReadOnlyAccess, AmazonRDSFullAccess
```

3. **EC2 + Tomcat**
```sh
sudo yum install tomcat -y
sudo systemctl start tomcat
sudo systemctl enable tomcat
```

4. **Application Load Balancer**
```text
- Create ALB with HTTP/HTTPS listeners
- Register EC2 targets
- Enable health checks
```

5. **RDS Deployment**
```text
- Launch DB in private subnet
- Enable Multi-AZ
- Allow EC2 access via SG
```

6. **RabbitMQ on EC2**
```sh
sudo yum install rabbitmq-server -y
sudo systemctl start rabbitmq-server
sudo systemctl enable rabbitmq-server
```

7. **Memcached on EC2**
```sh
sudo yum install memcached -y
sudo systemctl start memcached
sudo systemctl enable memcached
```

8. **Route 53 Setup**
```text
- Create Private Hosted Zone
- Add A/CNAME records for internal services and ALB
```


### 🛠️ Optional AWS Alternatives
| Service   | Alternative AWS Service | Description                     |
| --------- | ----------------------- | ------------------------------- |
| RabbitMQ  | **Amazon MQ**           | Fully managed ActiveMQ/RabbitMQ |
| Memcached | **ElastiCache**         | Managed Redis/Memcached caching |



### 📝 Summary

This project demonstrates how to lift and shift a monolithic web application to AWS using foundational services like EC2, RDS, VPC, ALB, and S3, while also incorporating message queuing and caching for better scalability and performance.
