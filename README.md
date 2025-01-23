Labour Hiring System - Deployment Documentation
Table of Contents
Introduction
System Overview
Technologies Used
Architecture
Kubernetes Deployment
Pipeline Workflow
Steps to Deploy Locally
Conclusion
Introduction
The Labour Hiring System is a web application designed to streamline the hiring process for laborers by connecting them with potential employers. This document provides step-by-step instructions to deploy the system using DevOps practices, including Docker, Kubernetes, and CI/CD pipelines with Jenkins.

System Overview
Features:
Admin Verification: Laborers are verified by an admin before being registered.
Search Functionality: Employers can search for verified laborers.
User-Friendly Interface: Easy-to-navigate frontend.
Components:
Frontend: Interface for users.
Backend: API for handling business logic and data communication.
MongoDB: Database for storing laborer and employer information.
Technologies Used
Docker: For creating containerized images of the application components.
Kubernetes: For orchestrating the deployment of the application.
Minikube: Local Kubernetes cluster for testing.
Jenkins: For setting up a CI/CD pipeline.
GitHub: Version control system for managing the source code.
kubectl: Command-line tool for managing Kubernetes resources.
Architecture
Three-Tier Architecture:
Frontend: Deployed as a Kubernetes pod, exposed via a LoadBalancer service.
Backend: Handles API requests, connected to MongoDB.
Database: MongoDB, deployed as a StatefulSet with PersistentVolume.
Kubernetes Components:
Namespace: Isolates the application resources.
Deployments: Manages pods for the frontend, backend, and database.
Services: Exposes pods for internal and external communication.
ConfigMaps and Secrets: Stores environment variables and sensitive data.
Kubernetes Deployment
Steps:
Namespace:

Apply namespace.yml to create a namespace for the project:
bash
Copy
Edit

> kubectl apply -f k8s/namespace.yml

MongoDB:

Deploy MongoDB using the deployment and service files:
bash
Copy
Edit

kubectl apply -f k8s/mongo/mongo-deployment.yml 

kubectl apply -f k8s/mongo/mongo-service.yml

Backend:

Deploy the backend using the deployment, service, and config files:
bash
Copy
Edit

kubectl apply -f k8s/backend/backend-deployment.yml 

kubectl apply -f k8s/backend/backend-service.yml 

kubectl apply -f k8s/backend/backend-config.yml 

Frontend:

Deploy the frontend using the deployment and service files:
bash
Copy
Edit

kubectl apply -f k8s/frontend/frontend-deployment.yml 

kubectl apply -f k8s/frontend/frontend-service.yml 

Pipeline Workflow
The Jenkins pipeline automates the process of building Docker images and deploying them to Kubernetes. Here’s the high-level overview of the pipeline:

Checkout Code:

Clones the GitHub repository to fetch the latest codebase.
Build Docker Images:

Builds separate images for:
Frontend
Backend
MongoDB
Pushes the images to Docker Hub.
Deploy to Kubernetes:

Applies Kubernetes manifests to deploy the application in Minikube.
Key Environment Variables:

> DOCKER_HUB_CREDENTIALS: Jenkins credentials for Docker Hub login.

> DOCKER_HUB_REPO: Docker Hub repository for storing the images.

> KUBECONFIG: Path to the kubeconfig file for accessing the Kubernetes cluster.

Steps to Deploy Locally
Prerequisites:
Install Docker, Minikube, and kubectl on your local machine.
Set up a Docker Hub account and configure Jenkins.
Deployment Steps:
Start Minikube:

bash
Copy
Edit

> minikube start

Build Docker Images:

bash
Copy
Edit

> docker build -t <docker-hub-repo-backend> ./backend

> docker build -t <docker-hub-repo-frontend> ./frontend 

> docker build -t <docker-hub-repo-mongo> ./mongo 
Push Images to Docker Hub:

bash
Copy
Edit

> docker push <docker-hub-repo-backend>

> docker push <docker-hub-repo-frontend>

> docker push <docker-hub-repo-mongo>

Apply Kubernetes Manifests:

bash
Copy
Edit

> kubectl apply -f k8s/namespace.yml

> kubectl apply -f k8s/mongo/mongo-deployment.yml

> kubectl apply -f k8s/mongo/mongo-service.yml

> kubectl apply -f k8s/backend/backend-deployment.yml

> kubectl apply -f k8s/backend/backend-service.yml

> kubectl apply -f k8s/backend/backend-config.yml

> kubectl apply -f k8s/frontend/frontend-deployment.yml

> kubectl apply -f k8s/frontend/frontend-service.yml 
Access the Application:

Use kubectl port-forward or minikube service to expose the frontend service.
Conclusion
The Labour Hiring System showcases the practical implementation of DevOps practices, emphasizing automation, containerization, and orchestration. The use of Jenkins for CI/CD pipelines and Kubernetes for deployment ensures scalability, reliability, and ease of management.
