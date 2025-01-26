# 🌐 Kubernetes Web Application with MongoDB

## 📝 Overview
This project demonstrates deploying a web application integrated with a MongoDB database using Kubernetes. It showcases the power of container orchestration, scalability, and efficient resource management.

---

## 📑 Table of Contents
- [🌟 Prerequisites](#-prerequisites)
- [🏗️ Architecture](#️-architecture)
- [⚙️ Setup & Installation](#️-setup--installation)
  - [🐳 Docker Setup](#-docker-setup)
  - [☸️ Kubernetes Deployment](#️-kubernetes-deployment)
- [📤 Output](#-output)
- [🧹 Cleaning Up Resources](#-cleaning-up-resources)
- [📚 Conclusion](#-conclusion)
- [🙏 Acknowledgment](#-acknowledgment)

---

## 🌟 Prerequisites
- 🖥️ Basic knowledge of Docker and Kubernetes.
- 🛠️ Tools: Docker, kubectl, and Minikube.

---

## 🏗️ Architecture
Below is the visual representation of the project architecture:

![Screenshot 2025-01-25 112605](https://github.com/user-attachments/assets/9188f4bb-04fa-43cd-befe-06610f1c97f3)

---
## 📝Project Outline 

![Screenshot 2025-01-25 112751](https://github.com/user-attachments/assets/8f775cd8-3bd7-440d-9239-6edd68c24fa1)


## ⚙️ Setup & Installation

### 🐳 Start a local Kubernetes cluster using Docker as the virtualization driver.
  Run following command.
  
  ```bash
  minikube start --driver docker
  ```
  ### ☸️ Kubernetes Deployment
  1. Apply the `mongo-config.yaml`:
     ```bash
     kubectl apply -f mongo-config.yaml
     ```
  2. Apply the `mongo-secret.yaml`:
     ```bash
     kubectl apply -f mongo-secret.yaml
     ```
  3. Deploy MongoDB using `mongo.yaml`:
     ```bash
     kubectl apply -f mongo.yaml
     ```
  4. Deploy the web application using `webapp.yaml`:
     ```bash
     kubectl apply -f webapp.yaml
     ```
  5. Access the web app:
     ```bash
     minikube service webapp-service
     ```

---

## 📤 Output
- The web application is successfully deployed and accessible.
- MongoDB serves as the database backend.

---

## 🧹 Cleaning Up Resources
Run the following commands to delete the Kubernetes resources:
```bash
kubectl delete -f webapp.yaml
kubectl delete -f mongo.yaml
kubectl delete -f mongo-secret.yaml
kubectl delete -f mongo-config.yaml
```
Stop Minikube:
```bash
minikube delete
```

---

## 📚 Conclusion
This project highlights the deployment of a containerized web application and database using Kubernetes. It demonstrates how to manage secrets, ConfigMaps, and scalable services.

---

## 🙏 Acknowledgment
Special thanks to [Nana Janashia](https://www.linkedin.com/in/nanajanashia/) for her mentorship and guidance in building this project.
