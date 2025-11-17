

---

# 📌 **README.md (Final Version)**

```md
# Paytm Full-Stack Recharge Application (AWS Deployment)

This project is a Paytm-like full-stack recharge application built using **HTML, CSS, JavaScript (Frontend)** and **Python Flask (Backend)**, fully deployed on AWS using **EC2, Apache, RDS MySQL, IAM Roles, and an Application Load Balancer**.

---

## 🚀 Project Architecture

The application follows a **3-tier architecture**:

### 🖥 1. Frontend (Presentation Layer)
- Built using **HTML, CSS, JavaScript**
- Hosted on an **EC2 instance**
- Served via **Apache Web Server**
- Sends API requests to backend using Load Balancer

### ⚙ 2. Backend (Application Layer)
- Developed using **Python Flask**
- Hosted on a separate **EC2 server**
- Exposed through an **Application Load Balancer**
- Handles:
  - User Signup
  - Login
  - Recharge Processing
  - Fetching Summary
- Connects to RDS using Python MySQL connector

### 🗄 3. Database Layer
- **AWS RDS MySQL**
- Stores:
  - User Information
  - Login Details
  - Recharge Transactions
  - Summary Data

---

## 🛠 Technologies Used

### **Frontend**
- HTML
- CSS
- JavaScript
- Apache Web Server (httpd)

### **Backend**
- Python
- Flask
- Flask-CORS
- MySQL Connector

### **AWS Services**
- EC2 (Frontend + Backend)
- RDS MySQL
- IAM Roles
- Application Load Balancer
- Security Groups

---

## 📂 Project Structure

```

Paytm-fullstack-project/
│
├── Backend/
│   ├── rds.py              # Flask backend logic + RDS connectivity
│   ├── paytm.sql           # Database tables
│   └── requirements.txt    # Backend dependencies
│
└── Frontend/
├── Frontend-code/      # HTML, CSS, JS (Static UI)
└── index.html
signup.html
login.html
recharge.html
summary.html
style.css
responsive.css

````

---

## ⚙ Backend Setup (EC2 + Flask)

```bash
sudo yum update -y
sudo yum install git -y
git clone https://github.com/<your-repo>/Paytm-fullstack-project.git

cd Paytm-fullstack-project/Backend

sudo yum install python3 -y
sudo yum install python3-pip -y
pip install -r requirements.txt
````

### Update database connection in `rds.py`:

```
host = "your-rds-endpoint"
user = "admin"
password = "your-password"
database = "paytm"
```

### Create database and tables:

```bash
mysql -h <rds-endpoint> -u admin -p < paytm.sql
```

### Run backend:

```bash
nohup python3 rds.py > rds.log 2>&1 &
```

---

## 🌐 Frontend Setup (EC2 + Apache)

```bash
sudo yum update -y
sudo yum install httpd -y
sudo systemctl start httpd
sudo systemctl enable httpd
```

### Deploy frontend:

```bash
cd Paytm-fullstack-project/Frontend/Frontend-code
sudo cp -r * /var/www/html/
```

### Update backend API URL inside:

* `signup.html`
* `login.html`
* `recharge.html`
* `summary.html`

Example:

```js
const API_URL = "http://<LOAD-BALANCER-URL>";
```

---

## 📡 Application Load Balancer

Backend EC2 is added inside a target group
Port: **5000**

Load Balancer forwards:

```
HTTP :80 → Backend port 5000
```

---

## ▶ How the Application Works

1. User opens frontend (Apache EC2)
2. HTML/JS sends API request → Backend Load Balancer
3. Flask receives request and validates data
4. Backend interacts with RDS MySQL
5. Data is inserted/fetched
6. Backend returns JSON response to frontend
7. Frontend displays results (Login, Recharge, Summary)

--
 ## ▶ Complete Flow Diagram#
User
│
├──▶ Opens Frontend URL
│        └─ Apache serves HTML/CSS/JS
│
├──▶ User clicks Signup/Login
│        └─ Frontend JS sends API call
│           to Load Balancer URL
│
├──▶ Load Balancer forwards request
│        └─ Target group (Backend EC2)
│
├──▶ Flask Backend receives request
│        ├─ Validates input
│        ├─ Connects to RDS MySQL
│        ├─ Executes SELECT/INSERT query
│        └─ Returns JSON response
│
├──▶ Frontend receives response
│        └─ Shows success or error message
│
├──▶ User performs Recharge
│        └─ Frontend → Backend API call
│
├──▶ Backend updates recharge table in RDS
│
└──▶ Summary Page
         └─ Backend fetches data from RDS
         └─ Frontend displays recharge summary

## ▶
