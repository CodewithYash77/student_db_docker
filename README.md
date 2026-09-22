Student Management Application — AWS + Docker + MySQL RDS

A full-stack Student Management Application deployed on an AWS EC2 Ubuntu server using Docker, with MySQL hosted on Amazon RDS.

The application is divided into:

Frontend — Vite-based web application

Backend — Spring Boot application

Database — Amazon RDS for MySQL

Deployment — Docker containers running on AWS EC2

1. Project Architecture

flowchart LR
    U[User / Browser] -->|HTTP :80| F[Frontend Container]
    F -->|REST API :8080| B[Backend Container]
    B -->|MySQL :3306| R[(Amazon RDS MySQL)]
    EC2[AWS EC2 Ubuntu] --> F
    EC2 --> B
    SG[Security Groups] -.controls access.-> EC2
    SG -.controls DB access.-> R

Request Flow

User
  ↓
EC2 Public IP :80
  ↓
Frontend Docker Container
  ↓
Backend API :8080
  ↓
Amazon RDS MySQL :3306
  ↓
student_db

2. AWS Resources Used

Resource

Purpose

Amazon EC2

Hosts the Docker containers

Ubuntu

Operating system of the EC2 server

Amazon RDS MySQL

Managed relational database

Security Group

Controls EC2 and RDS network access

Docker

Containerizes frontend and backend

Git

Downloads/updates application source code

Example EC2 Configuration

OS: Ubuntu

Instance type: t3.large / use the instance type actually selected

Storage: 15 GB or as required

Key Pair: Required for SSH access

Security Group: SSH + application ports

Keep the actual EC2 type and storage in sync with the instance you really used.

3. Required Ports

EC2 Security Group

Allow:

22    → SSH
80    → Frontend
8080  → Backend API

For learning/testing, these can be opened according to your lab requirement. For production, restrict access wherever possible.

RDS Security Group

Allow:

3306 → MySQL

Prefer allowing port 3306 from the EC2 Security Group, rather than opening MySQL to the whole internet.

4. Step 1 — Connect to EC2

SSH into the Ubuntu server:

ssh -i <key-file.pem> ubuntu@<EC2-PUBLIC-IP>

Update packages:

sudo apt update
sudo apt upgrade -y

5. Step 2 — Install Required Packages

Install Git:

sudo apt install git -y

Install MySQL client:

sudo apt install mysql-client -y

Install Docker:

sudo apt install docker.io -y

Start Docker:

sudo systemctl start docker

Enable Docker at boot:

sudo systemctl enable docker

Check Docker:

docker --version

Check Docker service:

sudo systemctl status docker

If required, use Docker with sudo:

sudo docker ps

6. Step 3 — Optional Docker Cleanup

Before starting a fresh lab deployment, existing containers/images can be checked.

List containers:

docker ps -a

List images:

docker images

To remove all existing containers:

docker rm -f $(docker ps -aq)

To remove all existing images:

docker rmi -f $(docker images -aq)

These cleanup commands are destructive. Use them only when you intentionally want to remove the existing Docker resources.

7. Step 4 — Create Amazon RDS MySQL Database

Create an Amazon RDS MySQL database.

Example configuration:

Engine: MySQL
Database name: student_db
Port: 3306
Username: admin
Password: <YOUR-DB-PASSWORD>

After RDS is created, copy the RDS endpoint.

Example:

<rds-endpoint>.ap-south-1.rds.amazonaws.com

Do not commit the real password to GitHub.

8. Step 5 — Test RDS Connection From EC2

Connect to the MySQL RDS instance:

mysql -h <RDS-ENDPOINT> -u admin -p

Enter the RDS password when prompted.

Check databases:

SHOW DATABASES;

Create the application database if it was not created during RDS setup:

CREATE DATABASE student_db;

Check again:

SHOW DATABASES;

Exit MySQL:

EXIT;

9. Step 6 — Clone the Git Repository

Clone the application:

git clone <YOUR-GITHUB-REPOSITORY-URL>

Enter the project directory:

cd <PROJECT-DIRECTORY>

Check files:

ls

Expected structure:

project/
├── backend/
└── frontend/

10. Step 7 — Configure Backend Database Connection

Go to backend:

cd backend

Open Spring Boot configuration:

vim src/main/resources/application.properties

Configure the database connection.

Example:

spring.datasource.url=jdbc:mysql://<RDS-ENDPOINT>:3306/student_db?useSSL=false&serverTimezone=UTC
spring.datasource.username=admin
spring.datasource.password=${DB_PASSWORD}

spring.jpa.hibernate.ddl-auto=update
spring.jpa.show-sql=true

Important

Replace:

<RDS-ENDPOINT>

with the actual RDS endpoint.

Do not put the real database password in a public Git repository.

For local/testing use, you can provide it through an environment variable:

export DB_PASSWORD='<YOUR-DB-PASSWORD>'

Then the Spring Boot application reads:

spring.datasource.password=${DB_PASSWORD}

11. Example JDBC URL

jdbc:mysql://<RDS-ENDPOINT>:3306/student_db

Meaning:

jdbc:mysql://
        ↓
MySQL protocol
        ↓
<RDS-ENDPOINT>
        ↓
RDS server
        ↓
3306
        ↓
MySQL port
        ↓
student_db
        ↓
Application database

12. Step 8 — Build Backend Docker Image

From the backend directory:

docker build -t <DOCKERHUB-USERNAME>/student-backend .

Example:

docker build -t myusername/student-backend .

Check image:

docker images

13. Step 9 — Run Backend Container

Run the backend:

docker run -d \
  --name backend \
  -p 8080:8080 \
  -e DB_PASSWORD='<YOUR-DB-PASSWORD>' \
  <DOCKERHUB-USERNAME>/student-backend

Check running containers:

docker ps

Check backend logs:

docker logs backend

Follow live logs:

docker logs -f backend

The backend should now be available through:

http://<EC2-PUBLIC-IP>:8080

14. Step 10 — Backend Troubleshooting

If the backend container is not running:

docker ps -a

Check logs:

docker logs backend

Check whether port 8080 is being used:

sudo ss -tulpn | grep 8080

Common issues:

RDS connection failed

Check:

RDS endpoint
RDS username
RDS password
RDS status
Security Group port 3306
EC2 → RDS network connectivity

Backend is running but browser cannot access it

Check:

EC2 Security Group → TCP 8080
Docker port mapping → 8080:8080
Application listening port → 8080

15. Step 11 — Configure Frontend

Go to the frontend directory:

cd ../frontend

Open the environment file:

vim .env

For a Vite application, configure:

VITE_API_URL=http://<EC2-PUBLIC-IP>:8080/api

Example:

VITE_API_URL=http://13.XXX.XXX.XXX:8080/api

Use the actual backend API path used by your application. If your backend endpoints do not contain /api, remove /api.

Check the frontend source before building to confirm the environment variable name and API path.

16. Step 12 — Build Frontend Docker Image

From the frontend directory:

docker build -t <DOCKERHUB-USERNAME>/student-frontend .

Example:

docker build -t myusername/student-frontend .

Check images:

docker images

17. Step 13 — Run Frontend Container

Run:

docker run -d \
  --name frontend \
  -p 80:80 \
  <DOCKERHUB-USERNAME>/student-frontend

Check:

docker ps

Open in browser:

http://<EC2-PUBLIC-IP>

18. Step 14 — Verify the Complete Application

Frontend

http://<EC2-PUBLIC-IP>

Backend

http://<EC2-PUBLIC-IP>:8080

Database

RDS MySQL
    ↓
student_db

The complete flow should be:

Browser
   ↓
Frontend :80
   ↓
Backend :8080
   ↓
RDS MySQL :3306
   ↓
student_db

19. Step 15 — Verify Data in MySQL

Connect to RDS:

mysql -h <RDS-ENDPOINT> -u admin -p

Select database:

USE student_db;

Check tables:

SHOW TABLES;

Check student/user data:

SELECT * FROM users;

Replace users with the actual table name created by your application.

20. Useful Docker Commands

Show running containers

docker ps

Show all containers

docker ps -a

Show images

docker images

Stop container

docker stop backend

Start container

docker start backend

Restart container

docker restart backend

Remove container

docker rm -f backend

View logs

docker logs backend

Follow logs

docker logs -f backend

21. Git Workflow

Check current branch:

git branch

Check changes:

git status

Pull latest code:

git pull origin main

Add files:

git add .

Commit:

git commit -m "Deploy student application using Docker and AWS RDS"

Push:

git push origin main

If the branch is new:

git push --set-upstream origin main

22. Recommended Git Repository Structure

student-management-app/
│
├── backend/
│   ├── src/
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

23. Important Files

Backend

backend/
├── src/
│   └── main/
│       ├── java/
│       └── resources/
│           └── application.properties
├── Dockerfile
└── pom.xml

Frontend

frontend/
├── src/
├── public/
├── Dockerfile
├── package.json
└── .env.example

24. Security — Do Not Push Secrets

Never commit:

DB password
AWS access keys
Private SSH keys
.pem files
Real production .env files
API keys
RDS credentials

Instead create:

.env.example

Example:

DB_PASSWORD=your_database_password
VITE_API_URL=http://your-ec2-ip:8080/api

The real .env should be ignored by Git.

25. Project Learning Outcomes

Through this project, I practiced:

AWS EC2 provisioning

Ubuntu server administration

Amazon RDS MySQL setup

MySQL database connectivity

Security Group configuration

Git repository cloning

Docker image creation

Docker container management

Backend container deployment

Frontend container deployment

Frontend-to-backend API configuration

Backend-to-RDS database connectivity

Application troubleshooting

Linux command-line operations

End-to-end cloud deployment

26. Technologies Used

AWS
├── EC2
├── RDS MySQL
└── Security Groups

Application
├── Frontend
├── Spring Boot Backend
└── MySQL Database

DevOps / Tools
├── Linux / Ubuntu
├── Docker
└── Git / GitHub

27. Final Deployment Checklist

[ ] EC2 launched
[ ] SSH connection working
[ ] Docker installed
[ ] Git installed
[ ] MySQL client installed
[ ] RDS MySQL created
[ ] student_db created
[ ] RDS endpoint verified
[ ] RDS Security Group configured
[ ] Backend cloned
[ ] application.properties configured
[ ] Backend Docker image built
[ ] Backend container running
[ ] Port 8080 allowed
[ ] Frontend API URL configured
[ ] Frontend Docker image built
[ ] Frontend container running
[ ] Port 80 allowed
[ ] Website accessible
[ ] Data inserted through application
[ ] Data verified in RDS
[ ] Secrets excluded from Git
[ ] README updated
[ ] Git commit created
[ ] GitHub push completed

28. One-Line Project Summary

Deployed a full-stack Student Management Application on AWS EC2 using Docker containers, connected the Spring Boot backend to Amazon RDS MySQL, configured networking through Security Groups, and exposed the frontend through the EC2 server.

Author

Yash Pawar

Cloud & DevOps | AWS | Linux | Docker | Git
