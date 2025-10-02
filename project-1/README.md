# <img src="https://github.com/devicons/devicon/blob/master/icons/amazonwebservices/amazonwebservices-original-wordmark.svg" alt="AWS" width="50"/> Aurora MySQL Database Setup

This lab sets up a **highly available Aurora MySQL cluster** within a **custom VPC architecture** in AWS using manual configuration (AWS Console). It's part of my DevOps learning journey, focusing on understanding cloud networking, security, and managed database services.

---

### ☁️ Resources Provisioned
The following components are created in this setup:

**1. Networking Layer**  
- **VPC** spanning multiple Availability Zones  
- **Public Subnets** for Bastion & NAT Gateways  
- **Private Subnets** for Aurora DB (Multi-AZ)  
- **NACLs & Security Groups** for traffic control  

**2. Access & Security**  
- **Bastion Host (Linux)** in Public Subnet for SSH access  
- **Security Groups** for DB access & isolation  

**3. Database Layer**  
- **Aurora MySQL Cluster** in Private Subnets  
- **Writer Instance** for primary DB operations  
- **Reader Instance (Replica)** in different AZ for HA and read scaling  

**4. Connectivity**  
- **NAT Gateways** allow outbound access from private DB subnets  
- Bastion → DB connection for admin access  


### 🔧 Project Architecture:

![Project Diagram](https://github.com/ahsan598/devops-projects-hands-on/blob/main/project-1-aws-aurora-db-setup/img/amazon-aurora-db-in-aws.svg)

---

### ⚙️ Implementation (Using AWS Console)

Here’s a step-by-step overview of how this setup was created:

1. **Create a VPC**
   - Use VPC Wizard or custom configuration
   - Include at least 2 public + 2 private subnets across different AZs

2. **Add Internet Gateway and route tables**
   - Attach IGW to VPC
   - Configure route tables for public subnets

3. **Create NAT Gateways**
   - One in each AZ for high availability
   - Allocate Elastic IPs for each

4. **Configure NACLs**
   - Allow required inbound/outbound rules for your subnets

5. **Launch Bastion Host**
   - Place in a public subnet
   - Associate a security group allowing SSH (port 22)

6. **Launch Aurora MySQL Cluster**
   - Choose "Amazon Aurora (MySQL-Compatible Edition)"
   - Place in private subnets only
   - Create a writer + replica instance (different AZs)
   - Use a new security group for DB (internal traffic only)

7. **Test Connectivity**
   - SSH into Bastion Host
   - Create an SSH tunnel to forward local port to Aurora DB endpoint


### 🔍 Testing the Deployment

To confirm that the Aurora MySQL DB is accepting connections:

1. SSH into the **bastion host** and set up **port forwarding**:
   ```bash
   ssh -i your-key.pem ec2-user@<Bastion-IP> -L 3306:<Aurora-Endpoint>:3306
   ```
2. Launch **MySQL Workbench** on your local machine.
3. Go to `Database > Connect to Database`.
4. Enter the following:
    - **Hostname**: `127.0.0.1`
    - **Port**: `3306` (or your forwarded port)
    - **Username**: Your `DBMasterUsername`
    - **Password**: Value entered during Aurora setup
5. Once connected, you’ll see the MySQL Workbench dashboard.
6. After testing, **terminate the SSH tunnel** using `Ctrl + C`

---

### 📝 Notes

- This setup was created manually to understand VPC-level networking and DB security zones.
- In future, this same setup can be automated using Terraform or AWS CLI for repeatability.
- Diagrams: Self-created using **draw.io/visualparadigm**.
