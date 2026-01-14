# spring-boot-CI-CD
Automated CI/CD pipeline for Spring Boot leveraging Jenkins, Maven, Docker, and GitHub Webhooks with large-scale container deployment using Docker Compose. 
# 🚀 Spring Boot CI/CD Pipeline using Jenkins & Docker

## 📌 Project Overview
This project demonstrates an **end-to-end CI/CD pipeline** for a **Spring Boot application** using **Jenkins, Maven, Docker, and GitHub Webhooks**.  
The pipeline automates code build, packaging, containerization, and deployment of **multiple Docker containers (scalable up to 100 instances)** on a Linux server.

This project follows **real-world DevOps practices** and is suitable for production-style deployments.

---

## 🛠️ Tech Stack
- **Backend:** Spring Boot (Java)
- **CI/CD Tool:** Jenkins (Declarative Pipeline)
- **Build Tool:** Maven
- **Containerization:** Docker, Docker Compose
- **Version Control:** Git, GitHub
- **Cloud Platform:** AWS EC2
- **Automation:** GitHub Webhooks

---

## 🔁 CI/CD Workflow
1. Developer pushes code to GitHub
2. GitHub Webhook triggers Jenkins automatically
3. Jenkins pipeline:
   - Pulls latest code
   - Builds application using Maven
   - Packages Spring Boot JAR
   - Builds Docker image
   - Deploys application using Docker containers
4. Application runs across multiple containers

---

## 📄 Jenkins Pipeline Stages
- **Checkout Source Code**
- **Build & Package using Maven**
- **Docker Image Build**
- **Docker Container Deployment**
- **Post-build Cleanup**

---

## 🐳 Docker & Deployment Features
- Dockerized Spring Boot application
- Multi-container deployment using Docker Compose
- Horizontal scaling supported (up to 100 containers)
- Clean container lifecycle management

---

## ⚙️ How to Run the Project

### 🔹 Prerequisites
- Java 17+
- Maven
- Docker & Docker Compose
- Jenkins
- Git

---

### 🔹 Clone the Repository
```bash
git clone https://github.com/your-username/springboot-jenkins-docker-cicd.git
cd springboot-jenkins-docker-cicd
