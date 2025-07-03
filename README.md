# 🚀 Jenkins CI/CD Pipeline with Docker & AWS EC2

A streamlined CI/CD pipeline using **Jenkins**, **Docker**, and **AWS EC2** instances for automated application deployment from development to production.

---

## 🛠️ Overview

This project automates the full deployment cycle:

- **Jenkins Master** triggers pipeline on code push.
- Uses **two AWS EC2 instances**:
  - One for **Test**
  - One for **Production**
- Remote deployment is handled via **SSH**, not Jenkins agents.
- Credentials securely stored using **Jenkins Secrets**.

---

## 🔁 Pipeline Stages

1. **Pull Code** from GitHub
2. **Build & Test** the application
3. **Create Docker Image** with built package
4. **Push Image** to Docker Hub
5. **Remote Deploy to Test** (AWS EC2 via SSH)
6. **Manual Approval**
7. **Remote Deploy to Production** (AWS EC2 via SSH)

---

## 🧰 Tech Stack

- **Jenkins**
- **GitHub**
- **Docker & Docker Hub**
- **AWS EC2**
- **SSH Authentication**
- **Jenkins Credentials Plugin**

---

## 🔐 Security

All sensitive data like:
- SSH private keys
- Docker Hub credentials  
are securely managed using **Jenkins Credential Store**.
