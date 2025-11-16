# AWS EC2 Setup for Jenkins CI/CD Environment
Quick guide to launch **3 Ubuntu** EC2 instances for **Jenkins, SonarQube, and Nexus**.


### Prerequisites
- AWS Account with appropriate permissions
- SSH key pair (create if not exists)
- Basic AWS console knowledge


### Step 1: Create Security Group & Key Pair

#### 1.1 Navigate to Security Groups
- Open **EC2 Dashboard → Security Groups**
- Click **Create security group**

#### 1.2 Configure Security Group

**Basic Details:**
- **Name:** `jenkins_devops_sg`
- **Description:** Security group for Jenkins, SonarQube, and Nexus
- **VPC:** (Default VPC)
- **Inbound Rules Configuration:**

    Type        |  Protocol  |  Port Range  |  Source     |  Description        
    ------------|------------|--------------|-------------|---------------------
    SSH         |  TCP       |  22          |  0.0.0.0/0  |  SSH access         
    HTTP        |  TCP       |  80          |  0.0.0.0/0  |  HTTP               
    HTTPS       |  TCP       |  443         |  0.0.0.0/0  |  HTTPS 
    SMTP        |  TCP       |  25          |  0.0.0.0/0  |  SMTP 
    SMTPS       |  TCP       |  465         |  0.0.0.0/0  |  SMTPS
    Custom TCP  |  TCP       |  6443        |  0.0.0.0/0  |  Kubernetes API
    Custom TCP  |  TCP       |  8080        |  0.0.0.0/0  |  Jenkins            
    Custom TCP  |  TCP       |  9000        |  0.0.0.0/0  |  SonarQube          
    Custom TCP  |  TCP       |  8081        |  0.0.0.0/0  |  Nexus Web          
    Custom TCP  |  TCP       |  8082        |  0.0.0.0/0  |  Nexus Docker       
    Custom TCP  |  TCP       |  3000-10000  |  0.0.0.0/0  |  Additional services
    
   <!-- ![Security Group Rules](/assets/imgs/security-group.png) -->

> **Security Note:** For production, restrict source IPs to your organization's IP range instead of `0.0.0.0/0`

- Click **Create security group**


#### 1.3 Create SSH Key Pair
- Navigate to Key Pairs
- Open **EC2 Dashboard → Network & Security → Key Pairs**
- Click **Create key pair** button

#### 1.4 Configure Key Pairs

**Fill the details::**
Field                    |  Value                                
-------------------------|---------------------------------------
Name                     |  `devops-key` (or your choice)           
Key pair type            |  RSA                                  
Private key file format  |  `.pem` (for Mac/Linux) or `.ppk` (for PuTTY)

> **Recommended Format:** `.pem` (works with SSH, Git Bash, PowerShell)

#### 1.5 Download Key
- Click **Create key pair**
- The `.pem` file will download automatically to your computer
- **Save it securely** - you can't download it again!

**Example:** `devops-key.pem`


### Step 2: Launch Multiple Instances

#### 2.1 Navigate to EC2 Dashboard

- **Launch Instance:**
  - Go to **EC2 Dashboard → Instances → Launch Instance**\
  - **Configure EC2 Instance:**

    Setting              |  Value                  
    ---------------------|-------------------------
    Name                 |  **Jenkins-Server, SonarQube-Server, Nexus-Server**         
    AMI                  |  **Ubuntu Server 24.04 LTS**
    Instance Type        |  **t2.medium**              
    Key Pair             |  Select **`devops-key.pem`**  
    Security Group       |  Select **`jenkins_devops_sg`**
    Storage              |  **25 GB gp3**              
    Number of Instances  |  **3**                     

- Click **Launch** Instance

#### 2.2 Rename Instances (Optional)

After launch, rename for clarity:
1. Go to **EC2 Dashboard** → **Instances**
2. Select each instance → Click pencil icon next to name
3. Rename:
   - Instance 1: `Jenkins-Server`
   - Instance 2: `SonarQube-Server`
   - Instance 3: `Nexus-Server`


### Step 3: Access Instances

#### 3.1 Get Public IPs
1. Go to EC2 Dashboard → Instances
2. Note down Public IPv4 addresses for all three servers:
   - Jenkins Server: <JENKINS_IP>
   - SonarQube Server: <SONAR_IP>
   - Nexus Server: <NEXUS_IP>

#### 3.2 Connect via SSH

Using Powershell / Git Bash / MobaXterm:
```sh
# Connect to Jenkins Server
ssh -i /path/to/key.pem ubuntu@<JENKINS_IP>

# Connect to SonarQube Server
ssh -i /path/to/key.pem ubuntu@<SONAR_IP>

# Connect to Nexus Server
ssh -i /path/to/key.pem ubuntu@<NEXUS_IP>
```

Fix Key Permissions (if needed):
```sh
chmod 400 /path/to/key.pem
```


### Verification Checklist

- [ ] 3 EC2 instances running
- [ ] Security group attached to all instances
- [ ] SSH access working
- [ ] Public IPs noted
- [ ] Docker installed on all servers


### Access URLs

After deployment:

| Service | Port | Access URL |
|---------|------|------------|
| Jenkins | 8080 | `http://<JENKINS_IP>:8080` |
| SonarQube | 9000 | `http://<SONAR_IP>:9000` |
| Nexus | 8081 | `http://<NEXUS_IP>:8081` |
| Nexus Docker | 8082 | `http://<NEXUS_IP>:8082` |


### Resource Summary

**Total AWS Resources:**
- 3 × EC2 t2.medium instances
- 1 × Security Group
- 3 × 25GB EBS volumes
- 1 × SSH Key Pair


### Cost Estimate:
- **Running:** Estimated Monthly Cost: ~$75-90 USD (varies by region and usage)
- **Stopped:** Stopped instances don't incur compute charges, only storage charges (~$3/month per instance)

> **Cost Saving Tip:** Stop instances when not in use to reduce compute charges


### Next Steps

1. **Install Jenkins** on Jenkins-Server
2. **Deploy SonarQube** container on SonarQube-Server
3. **Deploy Nexus** container on Nexus-Server
4. **Configure integrations** between services

Refer to individual installation guides for detailed setup instructions.


### Setup Complete! 🎉
You now have 3 separate servers for Jenkins, SonarQube, and Nexus ready for CI/CD pipeline configuration.
