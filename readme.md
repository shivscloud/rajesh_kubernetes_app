
---

# Flask Application Deployment on Kubernetes

This project demonstrates how to deploy a Flask application on a Kubernetes cluster using Docker. The application connects to a MongoDB instance and is configured with environment variables for flexibility. This README provides a comprehensive guide to setting up, deploying, and troubleshooting the application.
## Docker Image
# https://hub.docker.com/repository/docker/rajeshsingam/rajesh_singamsetti_app/general
# docker pull rajeshsingam/rajesh_singamsetti_app:latest

## Table of Contents

- [Prerequisites](#prerequisites)
- [Setup Instructions](#setup-instructions)
  - [Building and Tagging the Docker Image](#building-and-tagging-the-docker-image)
  - [Deploying to Kubernetes](#deploying-to-kubernetes)
  - [Accessing the Application](#accessing-the-application)
- [Troubleshooting](#troubleshooting)
  - [Verifying MongoDB Connectivity](#verifying-mongodb-connectivity)
  - [Troubleshooting Inside the Container](#troubleshooting-inside-the-container)
- [License](#license)

## Prerequisites 

Before starting, ensure you have the following installed:

- Docker
- Kubernetes (kubectl)
- A running Kubernetes cluster
- MongoDB deployed in the Kubernetes cluster

## Setup Instructions 

### ➡️ Building and Tagging the Docker Image

1. Build the Docker image for your Flask application:

   ```bash
   docker build -t app-flask-app:latest .
   ```

2. Tag the Docker image to push it to Docker Hub:

   ```bash
   docker tag app-flask-app:latest rajeshsingam/rajesh_singamsetti_app:v1
   ```

3. Push the tagged image to Docker Hub:

   ```bash
   docker push rajeshsingam/rajesh_singamsetti_app:v1
   ```

### ➡️ Deploying to Kubernetes 

1. Apply the Kubernetes deployment and service configurations:

   ```bash
   kubectl apply -f Deployment.yaml
   ```

2. Check the status of the deployment:

   ```bash
   kubectl get pods
   ```

3. Once the pods are running, access the application via the NodePort service:

   ```bash
   kubectl get svc flask-service
   ```

   Note the `NodePort` to access your application in a browser.

### ➡️ Accessing the Application 

Open your web browser and navigate to `http://<NodeIP>:<NodePort>` to access the Flask application.

## Troubleshooting 
### ➡️ Verifying MongoDB Connectivity

To verify MongoDB connectivity from within the Flask container, execute the following command:

```bash
kubectl exec -it <flask-pod-name> -- curl -v mongo-service:27017
```

Replace `<flask-pod-name>` with the actual pod name.

### ➡️ Troubleshooting Inside the Container 

If you need to troubleshoot inside the Flask container, follow these steps:

1. Access the running Flask pod:

   ```bash
   kubectl exec -it <flask-pod-name> -- /bin/bash
   ```

2. Inside the container, you can use the following commands:

   ```bash
   # List files
   ls

   # Ping MongoDB service
   ping mongo-service

   # Exit the container
   exit

   # Update package lists (if necessary)
   apt update

   # Install ping utility
   apt install iputils-ping

   # Install net-tools
   apt install net-tools

   # Use curl to check MongoDB connection
   curl -v mongo-service:27017

   # View command history
   history
   ```


## License 

This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for details.
