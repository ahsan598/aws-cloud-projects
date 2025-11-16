# AWS EC2 Setup — Ubuntu Server

A quick guide to launch an **Ubuntu** based EC2 instance on AWS.


### Prerequisites
- AWS Account
- SSH key pair (or create a new one)
- Basic AWS console knowledge


## Steps to Launch an EC2 Instance

### Step-1: Create Security Group
**1.1 Navigate to Security Group section**
- Go to **EC2 Dashboard → Security Groups**
- Click **Create** security group

**1.2 Configure Security Group**
- **Name:** `ubuntu_server_sg`
- **Description:** Security group for Ubuntu EC2 instance
- **VPC:** Default VPC
- **Inbound Rules:**
   | Type  | Protocol | Port Range | Source    | Description        |
   | ----- | -------- | ---------- | --------- | ------------------ |
   | SSH   | TCP      | 22         | 0.0.0.0/0 | SSH access         |
   | HTTP  | TCP      | 80         | 0.0.0.0/0 | Web (optional)     |
   | HTTPS | TCP      | 443        | 0.0.0.0/0 | Web SSL (optional) |
- Click **Save**


### Step-2: Create SSH Key Pair
**2.1 Navigate to Key Pairs Section**
- Go to **EC2 Dashboard → Network & Security → Key Pairs**
- Click **Create** key pair


**2.2 Configure Key Pairs**
| Field         | Value                |
| ------------- | -------------------- |
| Name          | `ubuntu-server-key`  |
| Key Pair Type | RSA                  |
| File Format   | `.pem` (recommended) |

Download and save the key securely.


### Step-3: Launch an EC2 Instance

**3.1 Navigate to EC2 Dashboard**
- Go to **EC2 Dashboard → Instances → Launch Instance**
- Instance Configuration
   | Setting        | Value                             |
   | -------------- | --------------------------------- |
   | Name           | `Ubuntu-Server`                   |
   | AMI            | **Ubuntu Server 24.04 LTS**       |
   | Instance Type  | **t2.micro** (Free-tier eligible) |
   | Key Pair       | `ubuntu-server-key.pem`           |
   | Security Group | `ubuntu_server_sg`                |
   | Storage        | 10GB gp3                          |
- Click **Launch** Instance


### Step-4: Access Instances
**4.1 Get Public IP**
- In **EC2 Dashboard → Instances**
- Copy the Public IPv4 Address of your instance

**4.2 Connect via SSH**
- Open Powershell / Git Bash
```sh
# Connect to Jenkins Server
chmod 400 /path/to/ubuntu-server-key.pem

ssh -i /path/to/ubuntu-server-key.pem ubuntu@<PUBLIC_IP>
```

**4.3 After Login Update System**
```sh
sudo apt update && sudo apt upgrade -y

# Install Basic Tools
sudo apt install -y curl wget git unzip
```


**Your Ubuntu EC2 instance is now ready for general use or further configuration.**
