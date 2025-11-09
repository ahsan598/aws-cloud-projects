# High-Availability Aurora MySQL Cluster on AWS


## 🎯 Project Overview
**Goal:** Deploy a production-style, highly available **Aurora MySQL cluster** inside a secured AWS VPC, using only the AWS Console.

**Main Focus:** Core AWS networking concepts, secure DB design, and hands-on multi-AZ deployment (manual, not IaC).


## Resources & Architecture

| Layer          | Components (multi-AZ where relevant)                              |
| -------------- | ----------------------------------------------------------------- |
| **Networking** | VPC, public/private subnets, IGW, NAT, NACLs, route tables        |
| **Security**   | Bastion hosts (public), EC2 key pair, Security Groups for SSH/DB  |
| **Database**   | Aurora MySQL cluster (writer + reader/replica in private subnets) |
| **Access**     | SSH tunneling via Bastion for DB admin                            |


### Architecture Diagram:
- **Public subnets:** Bastion hosts, NAT gateways
- **Private subnets:** Aurora MySQL DB & replica, only internal traffic allowed
- **Multi-AZ:** Everything spread over 2+ AZs for HA
- Central route table, custom NACLs, security groups

![HA-Aurora-Architecture](/assets/images/project-1.png)


### 1. Networking (VPC & Subnets)
- Create **VPC** with at least 2 public + 2 private subnets in different AZs.
- Attach **Internet Gateway (IGW)** and define proper route tables:
  - **Public subnets:** Allow internet access, host Bastion + NAT
  - **Private subnets:** For DBs, routed via NAT for outbound only

### 2. NAT & Security
- **NAT Gateways:** Deploy in each public subnet (1/NAT per AZ for real HA)
- **NACLs:** Control wider subnet access (optional for extra lockdown)
- **Security Groups:** Fine-grained access (Port `22` for SSH, `3306` between Bastion/DB)
- **Bastion Host:** Single, small Linux VM in public subnet, SSH allowed from trusted IPs only

### 3. Aurora MySQL Cluster
- Launch Aurora with:
  - **Writer (primary) instance** in Private Subnet A
  - **Replica (reader) instance** in Private Subnet B
- DB security group should allow inbound only from Bastion (or relevant EC2s)
- **No public IP** for Aurora nodes


## 🔍 Connect & Test
**1. SSH Tunnel via Bastion:**
```sh
ssh -i my-key.pem ubuntu@<Bastion-IP> -L 3306:<Aurora-Endpoint>:3306
```

**2. mysql/MySQL Workbench:**
- Host: `127.0.0.1`
- Port: `3306`
- Username/Password: (from Aurora setup)

3. **Validate:** Login and run a simple query to confirm connection.
4. Disconnect tunnel (`Ctrl+C`) when done.


## 📝 Notes & Learnings
- **Manual-first:** This setup was done via the AWS Console for true infrastructure basics learning — no automation/IaC yet.
- **Security:** Only Bastion can reach Aurora. Subnets/NACLs and SGs provide layered defense.
- **HA/DR:** Aurora is automatically multi-AZ for resilience.
