# 🌐 Hosting a Web Application with MongoDB on Kubernetes

## Overview
This project demonstrates how to deploy a web application connected to a MongoDB database using Kubernetes. The deployment utilizes Kubernetes manifest files to manage configuration, sensitive data, and services. The project leverages Minikube for a local Kubernetes cluster and highlights practical use cases such as managing ConfigMaps, Secrets, and NodePort services. It is guided by Nana Janashia.

---

## Table of Contents
1. [Prerequisites](#prerequisites)
2. [Architecture](#architecture)
3. [Setup & Installation](#setup--installation)
4. [Docker Setup](#docker-setup)
5. [Kubernetes Deployment](#kubernetes-deployment)
6. [Output](#output)
7. [Cleaning Up Resources](#cleaning-up-resources)
8. [Conclusion](#conclusion)
9. [Acknowledgment](#acknowledgment)

---

## Prerequisites

### Tools and Knowledge
- **Minikube**: Local Kubernetes cluster setup.
- **kubectl**: Kubernetes command-line tool.
- **Docker**: Containerization platform.

### Knowledge Requirements
- Basics of Kubernetes (Pods, Deployments, Services).
- Familiarity with YAML configuration files.

---

## Architecture

### Visual Representation
```
+---------------------------------+
|         Web Application         |
|         (Node.js)               |
+---------------------------------+
               |
               |
               v
+---------------------------------+
|       MongoDB Database          |
|         (Persistent Store)      |
+---------------------------------+
```

### Components
1. **Web Application**: Hosted on Kubernetes using a Deployment and exposed via NodePort Service.
2. **MongoDB**: Managed through Deployment and accessible via a ClusterIP Service.
3. **ConfigMaps and Secrets**: Store environment variables and sensitive data.

---

## Setup & Installation

### 1. Installing Tools
1. Install Minikube:
   ```bash
   curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
   chmod +x minikube-linux-amd64
   sudo mv minikube-linux-amd64 /usr/local/bin/minikube
   ```

2. Install kubectl:
   ```bash
   curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
   chmod +x kubectl
   sudo mv kubectl /usr/local/bin/
   ```

3. Start Minikube:
   ```bash
   minikube start --vm-driver=hyperkit
   ```

### 2. Check Minikube Status
```bash
minikube status
```

### 3. Retrieve Minikube IP
```bash
minikube ip
```

---

## Docker Setup

### Dockerfile Example
```dockerfile
FROM node:14
WORKDIR /app
COPY . .
RUN npm install
CMD ["node", "server.js"]
EXPOSE 3000
```

### Build, Run, and Push Docker Image
```bash
docker build -t webapp:v1 .
docker run -d -p 3000:3000 webapp:v1
docker tag webapp:v1 your_dockerhub_account/webapp:v1
docker push your_dockerhub_account/webapp:v1
```

---

## Kubernetes Deployment

### Manifest Files

#### mongo-config.yaml
```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: mongo-config
data:
  mongo-url: mongo-service
```

#### mongo-secret.yaml
```yaml
apiVersion: v1
kind: Secret
metadata:
  name: mongo-secret
type: Opaque
data:
  mongo-user: bW9uZ291c2Vy
  mongo-password: bW9uZ29wYXNzd29yZA==
```

#### mongo.yaml
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: mongo-deployment
  labels:
    app: mongo
spec:
  replicas: 1
  selector:
    matchLabels:
      app: mongo
  template:
    metadata:
      labels:
        app: mongo
    spec:
      containers:
      - name: mongodb
        image: mongo:5.0
        ports:
        - containerPort: 27017
        env:
        - name: MONGO_INITDB_ROOT_USERNAME
          valueFrom:
            secretKeyRef:
              name: mongo-secret
              key: mongo-user
        - name: MONGO_INITDB_ROOT_PASSWORD
          valueFrom:
            secretKeyRef:
              name: mongo-secret
              key: mongo-password
---
apiVersion: v1
kind: Service
metadata:
  name: mongo-service
spec:
  selector:
    app: mongo
  ports:
    - protocol: TCP
      port: 27017
      targetPort: 27017
```

#### webapp.yaml
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: webapp-deployment
  labels:
    app: webapp
spec:
  replicas: 1
  selector:
    matchLabels:
      app: webapp
  template:
    metadata:
      labels:
        app: webapp
    spec:
      containers:
      - name: webapp
        image: nanajanashia/k8s-demo-app:v1.0
        ports:
        - containerPort: 3000
        env:
        - name: USER_NAME
          valueFrom:
            secretKeyRef:
              name: mongo-secret
              key: mongo-user
        - name: USER_PWD
          valueFrom:
            secretKeyRef:
              name: mongo-secret
              key: mongo-password
        - name: DB_URL
          valueFrom:
            configMapKeyRef:
              name: mongo-config
              key: mongo-url
---
apiVersion: v1
kind: Service
metadata:
  name: webapp-service
spec:
  type: NodePort
  selector:
    app: webapp
  ports:
    - protocol: TCP
      port: 3000
      targetPort: 3000
      nodePort: 30100
```

### Deploy Components
```bash
kubectl apply -f mongo-config.yaml
kubectl apply -f mongo-secret.yaml
kubectl apply -f mongo.yaml
kubectl apply -f webapp.yaml
```

---

## Output

- Access the web application:
  ```bash
  minikube service webapp-service
  ```
- Logs:
  ```bash
  kubectl logs {pod-name}
  ```
- Example:
  - MongoDB service is running and accessible by the web application.
  - Web application successfully displays data from the database.

---

## Cleaning Up Resources
```bash
kubectl delete -f webapp.yaml
kubectl delete -f mongo.yaml
kubectl delete -f mongo-secret.yaml
kubectl delete -f mongo-config.yaml
minikube stop
```

---

## Conclusion
This project demonstrated the deployment of a web application connected to a MongoDB database on a Kubernetes cluster using Minikube. By leveraging ConfigMaps, Secrets, and NodePort services, it provided practical insights into managing configurations and exposing services.

---

## Acknowledgment
Guided by [Nana Janashia](https://www.youtube.com/@TechWorldwithNana).
