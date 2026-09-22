# 🚀 Student Management Application — AWS + Docker + MySQL RDS

## 📌 Project Overview

This project is a full-stack Student Management Application deployed on AWS using Docker and Amazon RDS MySQL.

## 🏗️ Architecture

```text
User / Browser
      ↓
Frontend Docker Container
      ↓
Backend Docker Container
      ↓
Amazon RDS MySQL
      ↓
student_db
☁️ AWS Services Used
1. Amazon EC2

Used to host the Docker containers.

2. Amazon RDS

Used as the managed MySQL database.

3. Security Groups

Used to control network access.

🐳 Docker Deployment
Backend
docker build -t <username>/student-backend .
docker run -d --name backend -p 8080:8080 <username>/student-backend
Frontend
docker build -t <username>/student-frontend .
docker run -d --name frontend -p 80:80 <username>/student-frontend
🗄️ Database Configuration
MySQL
   ↓
RDS Endpoint
   ↓
Port 3306
   ↓
student_db
🔄 Application Flow
Browser
   ↓
Frontend :80
   ↓
Backend :8080
   ↓
RDS MySQL :3306
🔐 Security
Database password is not stored in GitHub.
.env files are added to .gitignore.
SSH access is controlled through Security Groups.
RDS MySQL access is restricted to the required source.
🧪 Verification
docker ps
docker images
docker logs backend

MySQL:

SHOW DATABASES;
USE student_db;
SHOW TABLES;
SELECT * FROM users;
📚 What I Learned
AWS EC2
Amazon RDS MySQL
Docker
Linux
Git & GitHub
Security Groups
Backend deployment
Frontend deployment
Database connectivity
Cloud networking
👨‍💻 Author

Yash Pawar

Cloud & DevOps | AWS | Linux | Docker | Git


**हे direct `README.md` मध्ये paste केल्यावर GitHub वर `#` heading मोठी, `##` त्यापेक्षा छोटी आणि `###` त्यापेक्षा छोटी दिसेल.**
ekach file madhe de

हो भाऊ 👍 एकाच README.md file मध्ये पूर्ण content देतो. खालील पूर्ण code direct README.md मध्ये copy-paste कर. GitHub वर headings proper मोठ्या दिसतील.

# 🚀 Student Management Application — AWS + Docker + MySQL RDS

A full-stack Student Management Application deployed on **AWS EC2** using **Docker**, with **MySQL hosted on Amazon RDS**.

---

## 📌 Project Overview

This project demonstrates the deployment of a full-stack student management application using cloud and DevOps technologies.

The application consists of:

- Frontend — Vite-based web application
- Backend — Spring Boot application
- Database — Amazon RDS MySQL
- Deployment — Docker containers
- Server — AWS EC2 Ubuntu
- Version Control — Git & GitHub

---

# 🏗️ Project Architecture

```text
                    ┌─────────────────┐
                    │   User / Browser │
                    └────────┬────────┘
                             │
                             │ HTTP :80
                             ▼
                  ┌──────────────────────┐
                  │   AWS EC2 - Ubuntu   │
                  │                      │
                  │ ┌──────────────────┐ │
                  │ │ Frontend Docker  │ │
                  │ │    Container     │ │
                  │ │      :80         │ │
                  │ └────────┬─────────┘ │
                  │          │            │
                  │          │ API :8080  │
                  │          ▼            │
                  │ ┌──────────────────┐ │
                  │ │ Backend Docker   │ │
                  │ │    Container     │ │
                  │ │      :8080       │ │
                  │ └────────┬─────────┘ │
                  └──────────┼───────────┘
                             │
                             │ MySQL :3306
                             ▼
                  ┌──────────────────────┐
                  │   Amazon RDS MySQL   │
                  │                      │
                  │     student_db       │
                  └──────────────────────┘
🔄 Application Flow
User / Browser
      ↓
AWS EC2 Public IP :80
      ↓
Frontend Docker Container
      ↓
Backend Docker Container :8080
      ↓
Amazon RDS MySQL :3306
      ↓
student_db
☁️ AWS Services Used
AWS Service	Purpose
Amazon EC2	Hosts Docker containers
Amazon RDS	Managed MySQL database
Security Groups	Controls inbound/outbound access
VPC	Provides networking environment
Subnets	Provides network segmentation
💻 EC2 Configuration

Example configuration:

Operating System : Ubuntu
Instance Type    : t3.large
Storage          : 15 GB
Database         : Amazon RDS MySQL

Use the actual EC2 instance type and storage configured in your AWS account.

🔐 Security Group Configuration
EC2 Security Group

Allow the required ports:

22    → SSH
80    → Frontend
8080  → Backend API
RDS Security Group

Allow:

3306 → MySQL

For better security, allow MySQL access from the EC2 Security Group instead of opening port 3306 to the entire internet.

🖥️ Step 1 — Connect to EC2

Connect to the Ubuntu EC2 instance using SSH:

ssh -i <key-file.pem> ubuntu@<EC2-PUBLIC-IP>

Update the system:

sudo apt update
sudo apt upgrade -y
🐳 Step 2 — Install Docker

Install Docker:

sudo apt install docker.io -y

Start Docker:

sudo systemctl start docker

Enable Docker:

sudo systemctl enable docker

Check Docker version:

docker --version

Check Docker service:

sudo systemctl status docker
🔧 Step 3 — Install Git

Install Git:

sudo apt install git -y

Check Git:

git --version
🗄️ Step 4 — Install MySQL Client

Install MySQL client:

sudo apt install mysql-client -y

Check installation:

mysql --version
🧹 Step 5 — Check Existing Docker Resources

Check running containers:

docker ps

Check all containers:

docker ps -a

Check Docker images:

docker images

If you intentionally want to remove all existing containers:

docker rm -f $(docker ps -aq)

Remove all Docker images:

docker rmi -f $(docker images -aq)

⚠️ These commands are destructive. Use them only when you want to clean the Docker environment completely.

🗃️ Step 6 — Create Amazon RDS MySQL Database

Create an Amazon RDS MySQL database.

Example configuration:

Engine        : MySQL
Database      : student_db
Port          : 3306
Username      : admin
Password      : <YOUR-DB-PASSWORD>

After creating the RDS database, copy the RDS endpoint.

Example:

student-db.xxxxxxxxx.ap-south-1.rds.amazonaws.com
🔗 Step 7 — Connect EC2 to RDS MySQL

From the EC2 server:

mysql -h <RDS-ENDPOINT> -u admin -p

Enter the database password.

Check databases:

SHOW DATABASES;

Create the database if required:

CREATE DATABASE student_db;

Check again:

SHOW DATABASES;

Exit MySQL:

EXIT;
🧪 Step 8 — Test RDS Connection

Connect again:

mysql -h <RDS-ENDPOINT> -u admin -p

Then:

USE student_db;

Check tables:

SHOW TABLES;
📥 Step 9 — Clone Git Repository

Clone the project:

git clone <YOUR-GITHUB-REPOSITORY-URL>

Check files:

ls

Enter project directory:

cd <PROJECT-DIRECTORY>

Expected structure:

student-management-app/
│
├── backend/
└── frontend/
⚙️ Step 10 — Configure Backend

Go to backend:

cd backend

Open Spring Boot configuration:

vim src/main/resources/application.properties

Configure database connection:

spring.datasource.url=jdbc:mysql://<RDS-ENDPOINT>:3306/student_db?useSSL=false&serverTimezone=UTC
spring.datasource.username=admin
spring.datasource.password=${DB_PASSWORD}

spring.jpa.hibernate.ddl-auto=update
spring.jpa.show-sql=true
🔑 Database Password

Set the password using an environment variable:

export DB_PASSWORD='<YOUR-DB-PASSWORD>'

This avoids directly storing the database password inside GitHub.

🔗 JDBC Connection

The database connection follows this format:

jdbc:mysql://<RDS-ENDPOINT>:3306/student_db

Meaning:

jdbc:mysql
     ↓
MySQL Database
     ↓
RDS Endpoint
     ↓
Port 3306
     ↓
student_db
🐳 Step 11 — Build Backend Docker Image

From the backend directory:

docker build -t <DOCKERHUB-USERNAME>/student-backend .

Example:

docker build -t myusername/student-backend .

Check image:

docker images
▶️ Step 12 — Run Backend Container

Run the backend container:

docker run -d \
--name backend \
-p 8080:8080 \
-e DB_PASSWORD='<YOUR-DB-PASSWORD>' \
<DOCKERHUB-USERNAME>/student-backend

Check container:

docker ps

Check backend logs:

docker logs backend

Follow live logs:

docker logs -f backend

Backend URL:

http://<EC2-PUBLIC-IP>:8080
🔍 Step 13 — Backend Troubleshooting

Check all containers:

docker ps -a

Check backend logs:

docker logs backend

Check port 8080:

sudo ss -tulpn | grep 8080

If backend cannot connect to RDS, check:

✓ RDS Endpoint
✓ RDS Username
✓ RDS Password
✓ RDS Status
✓ RDS Security Group
✓ EC2 Security Group
✓ Port 3306
✓ Database Name
🌐 Step 14 — Configure Frontend

Go to frontend:

cd ../frontend

Open environment file:

vim .env

Configure:

VITE_API_URL=http://<EC2-PUBLIC-IP>:8080/api

Example:

VITE_API_URL=http://13.XXX.XXX.XXX:8080/api

Use the actual API path configured in the backend.

🐳 Step 15 — Build Frontend Docker Image

From the frontend directory:

docker build -t <DOCKERHUB-USERNAME>/student-frontend .

Example:

docker build -t myusername/student-frontend .

Check images:

docker images
▶️ Step 16 — Run Frontend Container

Run:

docker run -d \
--name frontend \
-p 80:80 \
<DOCKERHUB-USERNAME>/student-frontend

Check:

docker ps
🌍 Step 17 — Open Application

Open the EC2 public IP in browser:

http://<EC2-PUBLIC-IP>

The frontend should load.

🔄 Complete Request Flow
                         INTERNET
                            │
                            ▼
                    ┌───────────────┐
                    │    Browser    │
                    └───────┬───────┘
                            │
                         Port 80
                            │
                            ▼
              ┌──────────────────────────┐
              │       AWS EC2            │
              │        Ubuntu            │
              │                          │
              │  ┌────────────────────┐  │
              │  │ Frontend Container │  │
              │  │       Port 80      │  │
              │  └──────────┬─────────┘  │
              │             │            │
              │          API Call         │
              │             │            │
              │             ▼            │
              │  ┌────────────────────┐  │
              │  │ Backend Container  │  │
              │  │      Port 8080     │  │
              │  └──────────┬─────────┘  │
              └─────────────┼────────────┘
                            │
                         Port 3306
                            │
                            ▼
                 ┌─────────────────────┐
                 │   Amazon RDS MySQL  │
                 │                     │
                 │     student_db      │
                 └─────────────────────┘
🗄️ Step 18 — Verify Database

Connect to RDS:

mysql -h <RDS-ENDPOINT> -u admin -p

Select database:

USE student_db;

Show tables:

SHOW TABLES;

Check data:

SELECT * FROM users;

Replace users with the actual table name used by the application.

🐳 Useful Docker Commands
Check Running Containers
docker ps
Check All Containers
docker ps -a
Check Images
docker images
Stop Container
docker stop backend
Start Container
docker start backend
Restart Container
docker restart backend
Remove Container
docker rm -f backend
View Logs
docker logs backend
Follow Logs
docker logs -f backend
📦 Docker Image and Container Concept
Dockerfile
    ↓
docker build
    ↓
Docker Image
    ↓
docker run
    ↓
Docker Container

Backend:

Backend Source Code
       ↓
   Dockerfile
       ↓
Student Backend Image
       ↓
Backend Container
       ↓
Port 8080

Frontend:

Frontend Source Code
       ↓
   Dockerfile
       ↓
Student Frontend Image
       ↓
Frontend Container
       ↓
Port 80
🔀 Git Workflow

Check Git status:

git status

Pull latest code:

git pull origin main

Add changes:

git add .

Commit:

git commit -m "Deploy student management application using Docker and AWS RDS"

Push:

git push origin main

For a new branch:

git push --set-upstream origin main
📁 Project Structure
student-management-app/
│
├── backend/
│   ├── src/
│   │   └── main/
│   │       ├── java/
│   │       └── resources/
│   │           └── application.properties
│   │
│   ├── Dockerfile
│   ├── pom.xml
│   └── README.md
│
├── frontend/
│   ├── src/
│   ├── public/
│   ├── Dockerfile
│   ├── package.json
│   └── README.md
│
├── .gitignore
└── README.md
🔐 Security Best Practices

Never push the following files or information to GitHub:

❌ Database Password
❌ AWS Access Keys
❌ Private SSH Keys
❌ .pem Files
❌ Production .env Files
❌ API Keys
❌ RDS Credentials

Use:

.env

for local secrets.

Add .env to .gitignore.

Create:

.env.example

Example:

DB_PASSWORD=your_database_password
VITE_API_URL=http://your-ec2-ip:8080/api
🧪 Final Testing Checklist
[✓] EC2 launched
[✓] SSH connection working
[✓] Ubuntu server ready
[✓] Docker installed
[✓] Git installed
[✓] MySQL client installed
[✓] RDS MySQL created
[✓] student_db created
[✓] RDS endpoint verified
[✓] RDS Security Group configured
[✓] EC2 Security Group configured
[✓] Backend cloned
[✓] Backend database configuration completed
[✓] Backend Docker image created
[✓] Backend container running
[✓] Port 8080 configured
[✓] Frontend API URL configured
[✓] Frontend Docker image created
[✓] Frontend container running
[✓] Port 80 configured
[✓] Website accessible
[✓] Backend connected to RDS
[✓] Data inserted through application
[✓] Data verified in MySQL
[✓] Secrets protected
[✓] README documented
[✓] Git commit created
[✓] GitHub push completed
📚 What I Learned

Through this project, I gained practical experience in:

AWS EC2
Amazon RDS MySQL
Ubuntu Linux Server
Docker
Docker Images
Docker Containers
Git & GitHub
Security Groups
MySQL Connectivity
Spring Boot Backend Deployment
Frontend Deployment
REST API Communication
Database Connectivity
Cloud Networking
Linux Server Administration
Application Troubleshooting
End-to-End Cloud Deployment
🛠️ Technologies Used
Cloud
├── AWS EC2
├── AWS RDS
├── AWS VPC
└── Security Groups

Operating System
└── Ubuntu Linux

Application
├── Frontend
├── Spring Boot
└── MySQL

DevOps
├── Docker
├── Git
└── GitHub
🎯 Project Objective

The main objective of this project was to understand how a real-world full-stack application can be deployed on AWS using Docker containers and connected to a managed MySQL database using Amazon RDS.

The project helped me understand the complete deployment flow:

Source Code
    ↓
GitHub
    ↓
AWS EC2
    ↓
Docker
    ↓
Frontend + Backend Containers
    ↓
Amazon RDS
    ↓
MySQL Database
📝 Project Summary

Developed and deployed a full-stack Student Management Application on AWS EC2 using Docker containers. Configured a Spring Boot backend to connect with Amazon RDS MySQL, deployed the frontend through Docker, configured Security Groups for secure network communication, and verified end-to-end application-to-database connectivity.

👨‍💻 Author
Yash Pawar

Cloud & DevOps | AWS | Linux | Docker | Git

AWS • DevOps • Linux • Docker • Git • Cloud Networking
⭐ Thank You for Visiting

If you found this project useful, feel free to explore the repository and connect with me.
