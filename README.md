# 🚀 EC2 Automated Deployment with Secure S3 Log Archival

This project automates the deployment of an application on AWS EC2 and securely archives logs to Amazon S3. It follows best DevOps practices like IAM-based access control, lifecycle policies, and clean automation scripts.

---

## 📌 Project Goals

- Launch an EC2 instance
- Install dependencies (Java 21)
- Clone and deploy an application from GitHub
- Upload EC2 logs to S3 automatically on shutdown
- Upload app logs to S3 after deployment
- Implement fine-grained IAM access controls (read-only vs upload-only)
- Add lifecycle policy to clean old logs
- Test permission boundaries with dedicated IAM roles

---

## 🛠️ Tools & Technologies Used

- **AWS EC2** – Virtual server for app deployment  
- **AWS S3** – Cloud storage for logs  
- **AWS IAM** – Role-based access control  
- **Bash Shell Script** – For automation  
- **Systemd** – To trigger scripts on shutdown  
- **AWS CLI** – To interact with AWS services  
- **Ubuntu Linux** – EC2 AMI  
- **Java 21** – App dependency

---

## 🧰 Step-by-Step Workflow

### 1. IAM Roles Created
| Role Name         | Permissions |
|------------------|-------------|
| `ReadOnlyS3Role` | `s3:ListBucket`, `s3:GetObject` |
| `S3UploadOnlyRole` | `s3:PutObject`, `s3:CreateBucket`, `s3:PutObjectAcl` |

---

### 2. EC2 Instance Setup
- Launched EC2 instance (Ubuntu) with **`S3UploadOnlyRole`** attached
- Installed Java 21 and AWS CLI

---

### 3. S3 Bucket Creation
- Created a private bucket: `sahil-log-bucket-2025-july`
- Applied a **lifecycle rule** to delete all logs after **7 days**

---

### 4. Automation Scripts

#### ✅ `upload-logs.sh`
- Uploads `/var/log/cloud-init.log` to S3 on shutdown
- Triggered using `systemd` service

#### ✅ `deploy.sh`
- Deploys the app
- Uploads `/opt/app/logs/` to S3
- Accepts `--stage` parameter for environment-based configuration (e.g., Dev, Prod)

#### ✅ (Optional) `cron` or `at`
- Can schedule future uploads or shutdown to save cost

---

### 5. Security Verification
- A new EC2 instance was launched with **`ReadOnlyS3Role`**
- Verified:
  - ✅ Can **list** logs in S3
  - ❌ Cannot **upload** logs (permission denied)

---

## 📁 Folder Structure

