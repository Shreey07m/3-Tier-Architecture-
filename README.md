# 🚀 3-Tier Web Architecture on AWS

This project demonstrates the deployment of a scalable and modular **3-tier web application architecture** using Amazon Web Services (AWS), following best practices for network isolation, security, and performance.

## 📌 Architecture Overview

The architecture is divided into three logical tiers:

- **Web Tier (Presentation Layer)**: Static frontend hosted on an EC2 instance using Nginx.
- **App Tier (Application Layer)**: Node.js backend hosted on a private EC2 instance, managed with PM2.
- **Database Tier (Data Layer)**: MySQL-based RDS instance in a private subnet.

```
**
## 🛠️ Technologies & AWS Services Used**

- **Frontend**: Node.js (React), Nginx
- **Backend**: Node.js + Express
- **Database**: Amazon RDS (MySQL)
- **Cloud Services**: EC2, VPC, S3, RDS, IAM, ELB (ALB), Target Groups, Subnets, Security Groups
- **Process Manager**: PM2
- **Connection**: AWS Session Manager (for secure EC2 access)

---
**
## 🔧 Setup Instructions**

**### 1. 📦 Clone the Repository**

```bash
git clone https://github.com/Shreey07m/3-Tier-Architecture-.git
```bash


### **2. 🪟 Setup Infrastructure on AWS**

- Create custom **VPC**, **public/private subnets**
- Define **Security Groups** for:
  - Load Balancers
  - Web EC2
  - App EC2
  - RDS
- Setup **IAM Role** for EC2 to access S3
- Upload your application code and configuration files (like `nginx.conf` and `dbConfig.js`) to **S3**


### **3. 🛑 Deploy Database Layer
**
- Launch **RDS MySQL** in private subnets
- Create DB: `three-tierdb`
- Credentials:
  - Username: `admin`
  - Password: `JeetGauri07`

- Setup the database and table:

```sql
CREATE DATABASE webappdb;
USE webappdb;
CREATE TABLE transactions (
  id INT NOT NULL AUTO_INCREMENT,
  amount DECIMAL(10,2),
  description VARCHAR(100),
  PRIMARY KEY(id)
); ```

**### 4. 🔌 Deploy App Tier**

- Launch **EC2 instance** in a private subnet
- Use **AWS Session Manager** to connect

Install **Node.js**, **PM2**, and **MySQL client**:

```bash
sudo yum install mysql -y
curl -o- https://raw.githubusercontent.com/avizway1/aws_3tier_architecture/main/install.sh | bash
nvm install 16
npm install -g pm2 ```

**### 📥 Copy Application Code from S3 and Run the Server
**
```bash
aws s3 cp s3://<bucket-name>/application-code/app-tier/ app-tier --recursive
cd app-tier
npm install
pm2 start index.js
pm2 save ```

**###🔍 Test the Health Endpoint**
```bash
curl http://localhost:4000/health ```

**### 5. 🔁 Internal Load Balancer**

- Create a **Target Group** on port `4000`
- Attach the **App EC2 instance** to the Target Group
- Create an **Internal Load Balancer** and associate it with the Target Group

**### 6. 🌐 Deploy Web Tier**

- Launch an **EC2 instance** in a **public subnet**

**#### 🛠️ Install Node.js, Nginx, and Pull Frontend Code from S3:**

```bash
curl -o- https://raw.githubusercontent.com/avizway1/aws_3tier_architecture/main/install.sh | bash
nvm install 16
aws s3 cp s3://<bucket-name>/application-code/web-tier/ web-tier --recursive
cd web-tier
npm install
npm run build
sudo amazon-linux-extras install nginx1 -y ```

**###⚙️ Configure and Start Nginx:**
```bash
cd /etc/nginx
sudo rm nginx.conf
sudo aws s3 cp s3://<bucket-name>/application-code/nginx.conf .
sudo service nginx restart
chmod -R 755 /home/ec2-user
sudo chkconfig nginx on ```

**✅ Access the frontend via the Web Tier EC2 instance’s public IP in your browser.**

