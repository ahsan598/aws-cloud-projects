# 💻 SSH into an AWS EC2 Instance

This guide explains how to securely SSH into an EC2 instance on AWS using your `.pem` key file.
It also lists common mistakes and their fixes so you can quickly troubleshoot.

## ⚙️ Prerequisites

**Before connecting:**
- You have launched an EC2 instance (**Ubuntu**).
- You downloaded the key pair (**.pem file**) when creating the instance.
- You know your instance’s Public **IPv4 address or Public DNS**.
- **Example:**
  ```vbnet
  Public IPv4: 13.233.123.45
  Public DNS: ec2-13-233-123-45.ap-south-1.compute.amazonaws.com
  ```

**Secure your key file**
- Make sure the key file has the correct permissions - If permissions are too open, SSH will refuse to use.
  ```sh
  chmod 400 my-key.pem
  ```


## 🚀 Steps to connect to your EC2 instance

### Step-1: Connect using Git Bash / PowerShell (Recommended)
Just open **Git Bash** or **PowerShell**, then run:
```sh
ssh -i "C:\path\to\my-key.pem" ubuntu@<Public-IP>

# Use the default Ubuntu user for Ubuntu instances
ssh -i "my-key.pem" ubuntu@13.233.123.45
```

**List of default EC2 usernames by Operating System**

| **OS / AMI**           | **Default Username**  |
| ---------------------- | --------------------- |
| **Amazon Linux**       | `ec2-user`            |
| **Ubuntu**             | `ubuntu`              |
| **CentOS**             | `centos`              |
| **RHEL**               | `ec2-user` or `root`  |
| **Debian**             | `admin` or `debian`   |
| **Fedora**             | `fedora`              |


### Step-2 — Connect using PuTTY
1. Open **PuTTYgen**
2. Click Load, select your `.pem` file
3. Click Save private key → this generates a `.ppk` file
4. Open PuTTY
   - Host Name: `ubuntu@<Public-IP>`
   - Go to Connection → SSH → Auth
   - Browse and select your `.ppk` file
5. Click Open → **done!**



## 🛠️ Troubleshooting & Common Errors

### 1. Permission denied (publickey)
**Cause:**
- Wrong username (e.g., using **ec2-user** for Ubuntu)
- Wrong or missing key file
- Incorrect key permissions

**Fix:**
- Use correct username → `ubuntu`
- Use the same key pair used during instance launch
- Run `chmod 400 my-key.pem`


### 2. Connection timed out
**Cause:**
- Port 22 blocked in Security Group or Network ACL
- Instance stopped or restarting
- You’re behind a firewall blocking SSH

**Fix:**
- Open port 22 in Security Group
- Restart instance if needed
- Test connection with:
  ```sh
  ping <instance-public-ip>
  ```


## 💡 Tips
- Never share your `.pem` file — it’s your private key.
- Keep your instance Security Group restricted to **your IP**.
- Use **Elastic IP** for a permanent public address.
