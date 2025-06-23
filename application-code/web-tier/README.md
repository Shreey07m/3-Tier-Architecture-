# Web tier

## 🌐 Web Tier - Frontend + Nginx

This is the presentation layer of the 3-tier architecture. It uses **React** (or static HTML/CSS/JS) and is served via **Nginx**.

## 📁 Folder: `web-tier/`

## 🛠️ Setup Instructions

### 1. Pull Code from S3

```bash
aws s3 cp s3://<bucket-name>/application-code/web-tier/ web-tier --recursive
cd web-tier
npm install
npm run build
```

### 2. Install & Configure Nginx

```bash
sudo amazon-linux-extras install nginx1 -y
cd /etc/nginx
sudo rm nginx.conf
sudo aws s3 cp s3://<bucket-name>/application-code/nginx.conf .
sudo service nginx restart
```

