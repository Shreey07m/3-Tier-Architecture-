# ⚙️ App Tier - Backend Service (Node.js)

This is the backend service of the 3-tier architecture, built with **Node.js** and connected to **MySQL RDS**.

## 📁 Folder: `app-tier/`

## 🚀 Features
- Node.js + Express server
- Connects to MySQL DB
- Exposes `/health` API
- Managed using PM2

## 🛠️ Setup Instructions

### 1. Install Dependencies

```bash
npm install
```

### 2. Update DB Configuration

Make sure your dbConfig.js file has correct RDS endpoint, username, and password.
```js
module.exports = {
  host: 'your-rds-endpoint',
  user: 'admin',
  password: 'Password',
  database: 'webappdb'
};
```

### 3. Run the Server

```bash
pm2 start index.js
pm2 save
```

### 4. Test API

```bash
curl http://localhost:4000/health
```

## 📦 Deployment Notes

- Should run on EC2 in a private subnet
- Access MySQL through VPC only



