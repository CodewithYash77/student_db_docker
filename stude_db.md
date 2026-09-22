# How to Connect MySQL Project Using Docker

## Step 1: Delete all Docker Images & Containers

For containers:

```bash
docker rm -f $(docker ps -aq)

For images:

docker rmi -f $(docker images -aq)
Step 2: Launch EC2 Instance

Launch Ubuntu EC2 instance.

Instance Type: t3.large
Key Pair: Select key pair
Security Group: Allow required ports
Storage: 15 GB
Step 3: Launch RDS Database

Create MySQL RDS database.

Engine: MySQL
Database: student_db
Username: admin
Password: <password>
Port: 3306
Step 4: Connect MySQL on Server

Install MySQL client:

sudo apt update
sudo apt install mysql-client -y

Connect with RDS:

mysql -h <RDS-ENDPOINT> -u admin -p

Check databases:

show databases;

Create database:

create database student_db;

Exit:

exit;
Step 5: Clone Git Repository
git clone <github-url>

Go inside project:

cd student-app-1k8s
Step 6: Backend

Go inside backend:

cd backend

Open configuration:

vim src/main/resources/application.properties

Change only database endpoint and password.

Example:

spring.datasource.url=jdbc:mysql://<RDS-ENDPOINT>:3306/student_db
spring.datasource.username=admin
spring.datasource.password=<password>
Step 7: Build Backend Docker Image
docker build -t <docker-username>/backend .

Build backend image.

Step 8: Run Backend Container
docker run -itd --name backend -p 8080:8080 <docker-username>/backend

Backend will run on:

<EC2-IP>:8080

Check Security Group and allow:

TCP 8080
MySQL 3306
Step 9: Frontend

Go to frontend:

cd ../frontend

Open .env:

vim .env

Configure API URL:

VITE_API_URL=http://<BACKEND-IP>:8080/api
Step 10: Build Frontend Docker Image
docker build -t <docker-username>/frontend .
Step 11: Run Frontend Container
docker run -itd --name frontend -p 80:80 <docker-username>/frontend
Step 12: Open Website

Open browser:

http://<EC2-IP>
Step 13: Check MySQL Data

Connect to MySQL:

mysql -h <RDS-ENDPOINT> -u admin -p

Then:

show databases;

use student_db;

select * from users;
Project Flow
Frontend
   ↓
Backend
   ↓
Amazon RDS MySQL
   ↓
student_db
Technologies Used
AWS EC2
AWS RDS
Docker
MySQL
Ubuntu
Git/GitHub
