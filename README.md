# Todo-List Node.js Application with DevOps Automation

## Project Overview

This repository contains a **Todo-List** web application built with Node.js, Express, EJS templating, and MongoDB for data persistence. The application allows users to create, update, and delete tasks with a user-friendly interface.

This project has been adapted and extended to include DevOps automation covering containerization, CI/CD pipelines, infrastructure provisioning, and deployment automation as part of a DevOps Internship Assessment.

---

## Assessment Breakdown & Solution Outline

### Part 1: Dockerize & CI Pipeline (30 points)
- **Repository:** [Original Todo-List-nodejs repo](https://github.com/Ankit6098/Todo-List-nodejs)
- **MongoDB Setup:** The app connects to your own MongoDB database (configured via `.env` file).
- **Dockerization:** The Node.js app is containerized with a Dockerfile.
- **CI Pipeline:** GitHub Actions workflow builds the Docker image and pushes it to a private Docker Registry (e.g., AWS ECR or Docker Hub).

### Part 2: VM Provisioning & Configuration with Ansible (30 points)
- **Linux VM:** Created locally or in the cloud.
- **Ansible Automation:** Ansible playbooks configure the VM, install Docker, and prepare the environment for container deployment.
- **Remote Execution:** Ansible runs from the local machine targeting the VM via SSH.

### Part 3: Container Orchestration & Auto-Update (40 points)
- **Docker Compose:** Used on the VM to run the application container.
- **Health Checks:** Configured in `docker-compose.yml` to monitor container health.
- **Image Auto-Update:** Continuous monitoring of the private Docker registry for updates to the image. Upon detection, the VM pulls the new image and restarts the container.  

### Part 4 - Bonus: Kubernetes & CD with ArgoCD 
- **Kubernetes:** Deploy the containerized application on the VM using Kubernetes instead of Docker Compose.
- **Continuous Deployment:** Use ArgoCD to automate the deployment workflow to the Kubernetes cluster for real-time updates from the image registry.

---

## Project Architecture

+-------------------+ +-------------------------+ +------------------------+
| Developer Machine | | CI/CD Pipeline on GitHub| | Cloud/Local Linux VM |
| (Local Dev & Git) +------------>+ GitHub Actions Workflow +------------>+ Docker & Orchestration |
+-------------------+ +-------------------------+ +------------------------+
| | |
| Source code & Dockerfile | Builds Docker image | Runs container
| | Pushes image to private registry (e.g., ECR) |
v v v
MongoDB database Docker Registry (AWS ECR or Docker Hub) Docker Compose or Kubernetes

---

## File Structure Summary

.
├── .github/
│ └── workflows/
│ └── ci-workflow.yaml # GitHub Actions workflow for CI pipeline
├── ansible/
│ ├── inventory # Defines target VM(s)
│ ├── playbook.yml # Ansible playbook for provisioning & setup
│ └── roles/ # Ansible roles for Docker and other dependencies
├── docker-compose.yml # Docker Compose file for local/VM deployment
├── Dockerfile # Docker instructions to containerize Node.js app
├── src/ # Source code of the Todo List app
├── .env # Environment variables including MongoDB URI
├── README.md # This file

---

## How to Use / Steps to Run

### 1. Clone Repo and Configure MongoDB

git clone https://github.com/Ankit6098/Todo-List-nodejs.git
cd Todo-List-nodejs

text

- Create and configure your own MongoDB database.
- Update the `.env` file with your MongoDB connection URI.

### 2. Dockerize the Application

- The `Dockerfile` builds the container with Node.js and app dependencies.
- Build and test the Docker image locally.

### 3. Set Up GitHub Actions CI Pipeline

- Configure GitHub Secrets for accessing your private Docker registry and AWS credentials (if using ECR).
- Workflow in `.github/workflows/ci-workflow.yaml` builds the Docker image and pushes it on every push to `main`.

### 4. Provision & Configure Linux VM with Ansible

- Create a Linux VM locally or in the cloud.
- Use the provided Ansible playbooks (`ansible/playbook.yml`) run from your local machine to:
  - Install Docker & Docker Compose.
  - Configure the environment for app deployment.
- Inventory file defines your VM's IP and SSH credentials.

### 5. Deploy & Manage Your App on the VM

- Use `docker-compose.yml` to run the app container on the VM.
- Docker Compose file includes health check configurations.
- Use a tool like **Watchtower** for monitoring image changes and automatic updates.

### 6. Bonus: Kubernetes & ArgoCD (Optional)

- Replace Docker Compose with Kubernetes manifests to deploy the app.
- Use ArgoCD for continuous deployment from your Git repo to Kubernetes cluster.

---

## Technologies & Tools Used

| Area                  | Technology                                       |
|-----------------------|------------------------------------------------|
| Backend               | Node.js, Express.js                             |
| Database              | MongoDB, Mongoose                              |
| Containerization      | Docker, Docker Compose                          |
| CI/CD                 | GitHub Actions                                 |
| Cloud Platform        | AWS (for ECR registry, optional)               |
| Infrastructure as Code| Ansible                                        |
| Container Orchestration| Docker Compose, Kubernetes (optional bonus)   |
| Continuous Deployment  | ArgoCD (optional bonus)                         |

---

## Additional Notes

- Ensure your AWS IAM user or Docker registry account has correct permissions to push images.
- Maintain security by storing sensitive keys in GitHub Secrets.
- Consider tagging docker images using Git SHA or semantic versioning for better tracking.
- Health checks in Docker Compose improve container reliability.
- For image auto-update, **Watchtower** is a popular and easy-to-use solution, but other tools like Kubernetes operators or custom scripts can be used depending on complexity.
- Kubernetes and ArgoCD require additional cluster setup and configuration beyond the scope of traditional Docker Compose.

---

## References and Resources

- [Todo-List-nodejs original repo](https://github.com/Ankit6098/Todo-List-nodejs)
- [Dockerizing Node.js & MongoDB - Tutorials](https://hevodata.com/learn/docker-nodejs-mongodb/)
- [GitHub Actions for Docker CI/CD](https://github.com/aws-actions/configure-aws-credentials)
- [Ansible documentation](https://docs.ansible.com/)
- [Watchtower - Automatic Docker Container Updater](https://github.com/containrrr/watchtower)
- [Kubernetes & ArgoCD Docs](https://argo-cd.readthedocs.io/en/stable/)

---

