# Containerized Flask App Deployment on AWS EKS with Kubernetes

This repository demonstrates how to containerize and deploy a Flask API to a Kubernetes cluster using **Docker**, **AWS EKS**, **CodePipeline**, and **CodeBuild**.

The project includes a simple API with three endpoints, containerized for scalability, and deployed to a production-ready environment using AWS tools. A CI/CD pipeline is implemented to automate testing, building, and deployment.

## Features

### Flask API Endpoints
- **GET `/`**: Health check endpoint, returns `Healthy`.
- **POST `/auth`**: Accepts an email and password as JSON arguments, returns a JWT based on a custom secret.
- **GET `/contents`**: Requires a valid JWT and returns the decrypted contents of the token.

### Deployment Highlights
- Containerized the Flask application with Docker.
- Deployed the container to Kubernetes on AWS EKS.
- Configured a CI/CD pipeline using AWS CodePipeline and CodeBuild.
- Automated secret management using AWS Parameter Store.
- Exposed the application publicly via a Kubernetes service.

## Prerequisites

To replicate this project, ensure you have the following installed and configured:

1. **Development Tools**:
   - Docker Desktop: [Install Docker](https://www.docker.com/products/docker-desktop/).
   - Git: [Download Git](https://git-scm.com/).
   - VS Code: [Install VS Code](https://code.visualstudio.com/).

2. **Python Environment**:
   - Python (Version 3.7 to 3.9): [Download Python](https://www.python.org/downloads/).
   - PIP (Version 19.x or higher):
     ```bash
     pip --version
     pip install --upgrade pip==20.2.3
     ```

3. **AWS Tools**:
   - AWS CLI: [Install AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html). Configure it with:
     ```bash
     aws configure
     aws configure set region us-east-2
     ```
   - `eksctl`: [Install eksctl](https://eksctl.io/introduction/installation/).
   - `kubectl`: [Install kubectl](https://kubernetes.io/docs/tasks/tools/).

## Project Objectives

The main objective is to deploy a Flask web application in a containerized environment to a Kubernetes cluster on AWS EKS, ensuring scalability and automation through CI/CD pipelines.

## Project Structure

```plaintext
.
├── Dockerfile                        # Containerization of Flask app
├── README.md                         # Documentation
├── aws-auth-patch.yml                # Kubernetes authentication config
├── buildspec.yml                     # AWS CodeBuild build specification
├── ci-cd-codepipeline.cfn.yml        # AWS CloudFormation template for CI/CD pipeline
├── iam-role-policy.json              # IAM policy for AWS CodeBuild/CodePipeline
├── main.py                           # Flask application entry point
├── requirements.txt                  # Python dependencies
├── simple_jwt_api.yml                # Kubernetes deployment configuration
├── test_main.py                      # Unit tests for the Flask app
└── trust.json                        # Trust relationship for IAM roles
```

## Steps to Deploy

### 1. Fork and Clone the Repository
Fork this repository to your GitHub account and clone it locally:
```bash
git clone https://github.com/<your-username>/Containerized-Flask-App-Deployment-on-AWS-EKS.git
cd Containerized-Flask-App-Deployment-on-AWS-EKS/
```

### 2. Build and Test the Container Locally
1. Build the Docker image:
   ```bash
   docker build -t flask-api:latest .
   ```
2. Run the container locally:
   ```bash
   docker run -p 8080:8080 flask-api:latest
   ```

### 3. Create and Configure an EKS Cluster
1. Create a Kubernetes cluster using `eksctl`:
   ```bash
   eksctl create cluster --name flask-cluster --region us-east-2
   ```
2. Apply Kubernetes manifests:
   ```bash
   kubectl apply -f simple_jwt_api.yml
   ```

### 4. Store Secrets in AWS Parameter Store
Add the JWT secret to AWS Parameter Store:
```bash
aws ssm put-parameter --name "JWT_SECRET" --value "<your-secret>" --type "SecureString"
```

### 5. Set Up CI/CD
1. Create a pipeline using `ci-cd-codepipeline.cfn.yml`.
2. Add build specifications in `buildspec.yml` for building, testing, and deploying.

## Outcome

- The Flask application is deployed on a Kubernetes cluster managed by AWS EKS.
- The application is publicly accessible via an external IP address.
- A CI/CD pipeline automates testing, building, and deployment.
