# Frontend Deployment and Service
### This repository contains Kubernetes manifests for deploying a frontend application using a Kubernetes Deployment and Service. 
### Additionally, it includes a Jenkins pipeline for building, running, pushing Docker images to Docker Hub, and deploying to an Amazon EKS cluster.
## Kubernetes Deployment
###This Deployment creates two replicas of the frontend application and exposes it via a NodePort service on port 8079.
## Jenkins Pipeline
###The Jenkins pipeline defined in the Jenkinsfile automates the build, run, and deployment processes. It performs the following stages:

## Jenkins Pipeline
###The Jenkins pipeline defined in the Jenkinsfile automates the build, run, and deployment processes. It performs the following stages:
## Jenkins Pipeline Stages

1. **Build:**
   Builds the Docker image for the frontend application.

2. **Run:**
   Runs the Docker image in a temporary network, ensuring a clean environment for each build.

3. **Push:**
   Tags and pushes the Docker image to Docker Hub, both with the specific build tag and as the latest version.

4. **Deploy EKS:**
   Deploys the application to an Amazon EKS cluster. This stage requires AWS credentials and EKS configuration stored securely in Jenkins credentials.

   Ensure you have the necessary Jenkins credentials set up for Docker Hub, AWS, and EKS before running the pipeline.

Feel free to customize the pipeline according to your specific requirements and deployment environment.


