# SSH into an EC2 Instance on AWS

This guide explains how to securely SSH into an EC2 instance on AWS using your `.pem` key file.
It also lists common mistakes and their fixes so you can quickly troubleshoot.

## Prerequisites

**Before connecting:**
- You have launched an EC2 instance (**Ubuntu**).
- You downloaded the key pair (**.pem file**) when creating the instance.
- You know your instance’s Public **IPv4 address or Public DNS**.

**Example:**
```vbnet
Public IPv4: 13.233.123.45
Public DNS: ec2-13-233-123-45.ap-south-1.compute.amazonaws.com
Key file: my-key.pem
```

## Secure your key file

Make sure the key file has the correct permissions (Linux/Mac):
```sh
chmod 400 my-key.pem
```

> If permissions are too open, SSH will refuse to use the key.


## Step-1: Connect to your instance

Use the default Ubuntu user for Ubuntu instances.
```sh
ssh -i "my-key.pem" ubuntu@ec2-13-233-123-45.ap-south-1.compute.amazonaws.com
```

or (if you prefer using the public IP):
```sh
ssh -i "my-key.pem" ubuntu@13.233.123.45
```

> - Amazon Linux? → use `ec2-user` instead of `ubuntu`
> - CentOS? → use `centos`
> - RHEL? → use `ec2-user` or `root`


## Step-2: For Windows users

### Option 1 — Using Git Bash or PowerShell (Recommended)

If you have **Git Bash** or **PowerShell** installed, you can connect easily using the same SSH command as Linux or macOS.

Just open **Git Bash** or **PowerShell**, then run:
```sh
ssh -i "C:\path\to\my-key.pem" ubuntu@<Public-IP>
```

**Explanation:**

- `ssh` → the command to start an SSH session
- `-i "C:\path\to\my-key.pem"` → specifies your private key file
- `ubuntu@<Public-IP>` → connects to your EC2 instance as the Ubuntu user

**Make sure:**
- The key file path is correct (use quotes if there are spaces in folder names)
- Port 22 is open in your EC2 security group
- You use the correct username (`ubuntu` for Ubuntu instances)

### Option 2 — Using Command Prompt

If you don’t have Git Bash or PowerShell with OpenSSH, you can still use **Windows Command Prompt** (on Windows 10+ where SSH is built-in).

**Run:**
```cmd
ssh -i "C:\path\to\my-key.pem" ubuntu@<Public-IP>
```

If you get an error like “***ssh not recognized***”, install **OpenSSH Client** from:
  - Settings → Apps → Optional Features → “***Add a feature***” → **OpenSSH Client**


### Option 3 — Using PuTTY
1. Open **PuTTYgen**
2. Click Load, select your `.pem` file
3. Click Save private key → this generates a `.ppk` file
4. Open PuTTY
   - Host Name: `ubuntu@<Public-IP>`
   - Go to Connection → SSH → Auth
   - Browse and select your `.ppk` file
5. Click Open → **done!**


## Step-3: Check your Security Group

Make sure inbound rules allow SSH:
| Type | Protocol | Port | Source                                    |
| ---- | -------- | ---- | ----------------------------------------- |
| SSH  | TCP      | 22   | Your IP or `0.0.0.0/0` (for testing only) |

> - Prefer “My IP” for security
> - “0.0.0.0/0” is fine for quick setup or testing


## Troubleshooting & Common Errors

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


### 3. Host key verification failed

**Cause:** SSH cached old key for same IP.

**Fix:**
```sh
ssh-keygen -R <instance-public-ip>
```

## Use Verbose Mode for Debugging (Optional)

If it still fails, run:
```sh
ssh -v -i "my-key.pem" ubuntu@<instance-public-ip>
```
This prints detailed logs of the SSH handshake.


## Quick Recap
| Step | Command / Action                         | Notes                 |
| ---- | ---------------------------------------- | --------------------- |
| 1    | `chmod 400 my-key.pem`                   | Fix key permissions   |
| 2    | `ssh -i "my-key.pem" ubuntu@<Public-IP>` | Connect to Ubuntu EC2 |
| 3    | Check Security Group                     | Port 22 open          |
| 4    | If issue: `ssh -v ...`                   | Debug mode            |


## Tips
- Never share your `.pem` file — it’s your private key.
- Keep your instance Security Group restricted to your IP.
- Use Elastic IP for a permanent public address.
- For automation, consider AWS Systems Manager Session Manager (no SSH key needed).
