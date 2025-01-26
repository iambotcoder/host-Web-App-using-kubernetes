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

```plaintext
+-------------------+
|   Internet/Browser  |
+-------------------+
         |
         v
+-------------------+
| Kubernetes Service |
+-------------------+
         |
         v
+-------------------+
|  Web Application   |
|  (Node.js-based)   |
+-------------------+
         |
         v
+-------------------+
|    MongoDB DB      |
+-------------------+
```
### Description:
- **Internet/Browser:** End-users access the web application via the browser.
- **Kubernetes Service:** Exposes the application to the external network using a NodePort service.
- **Web Application:** A containerized Node.js-based web app.
- **MongoDB Database:** Stores application data, configured using Kubernetes secrets and ConfigMaps.

---

## ⚙️ Setup & Installation
### 🐳 Docker Setup
1. Create a `Dockerfile` for the web application.
2. Build the Docker image:
   ```bash
   docker build -t webapp-image .
   ```
3. Run the Docker container:
   ```bash
   docker run -p 3000:3000 webapp-image
   ```
4. Push the Docker image to Docker Hub:
   ```bash
   docker tag webapp-image <dockerhub-username>/webapp-image
   docker push <dockerhub-username>/webapp-image
   ```

---

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
- Example screenshot or link: 🌐 [Web Application Output](#).

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
minikube stop
```

---

## 📚 Conclusion
This project highlights the deployment of a containerized web application and database using Kubernetes. It demonstrates how to manage secrets, ConfigMaps, and scalable services.

---

## 🙏 Acknowledgment
Special thanks to [Nana Janashia](https://www.linkedin.com/in/nanajanashia/) for her mentorship and guidance in building this project.
