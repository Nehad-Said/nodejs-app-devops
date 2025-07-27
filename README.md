⚙️ DevOps Architecture
mermaid
Copy
Edit
flowchart TD
    subgraph Developer Laptop
        A1[Code Commit] -->|Triggers| GH[GitHub Actions]
    end

    subgraph GitHub
        GH -->|Builds Docker Image| CI[CI Pipeline]
        CI -->|Push to Registry| REG[Private Docker Registry]
        GH -->|Sync App Manifests| GIT[GitOps Repo]
    end

    subgraph Cloud Infrastructure (EC2)
        VM[EC2 Instance (Ubuntu)]
        VM -->|Provision| Ansible[Ansible]
        VM -->|Run on K8s| K8s[Kubernetes Cluster]
    end

    GIT -->|Monitors Git Repo| Argo[ArgoCD]
    REG -->|Checks for Image Tags| Updater[ArgoCD Image Updater]

    Updater -->|Update app manifests| GIT
    Argo -->|Sync app to K8s| K8s
🛠️ Workflow Breakdown
✅ Part 1 – CI Pipeline (GitHub Actions)
Trigger on push to main

Build Docker image

Push to private Docker registry (AWS ECR)


✅ Part 2 – Provision EC2 VM
Launch EC2 (Ubuntu)

Run Ansible from local:

Install Docker

Install kubeadm, kubectl, kubelet

Set up a single-node Kubernetes cluster

Install ArgoCD and ArgoCD Image Updater

✅ Part 3 – CD with ArgoCD + Auto Update with Image Updater
ArgoCD watches Git repo and applies Kubernetes manifests

ArgoCD Image Updater monitors AWS ECR

When new image version is pushed:

Image Updater patches deployment manifest 

Commits it to Git or updates it directly in K8s 

ArgoCD syncs the updated workload

🔁 CI/CD Flow Summary
Developer pushes code → GitHub

GitHub Actions:

Builds Docker image

Tags and pushes image to registry


ArgoCD Image Updater:

Detects new image

Updates K8s manifest

ArgoCD:

Syncs updated manifests to Kubernetes cluster

App is redeployed with new image

💡 Technologies & Justification
Tool	Purpose	Reason
Backend	Node.js, Express.js
Database	MongoDB, Mongoose
Containerization	Docker, Docker Compose
GitHub Actions	CI	Native, easy to integrate
Docker Registry	Store built images	Use ECR
Ansible	Provision EC2	Agentless, powerful automation
Kubernetes	Deployment environment	Scalable and production-ready
ArgoCD	Continuous Delivery	GitOps-based, easy rollback/sync
ArgoCD Image Updater	Auto image pull	Clean, native Argo integration

Additional Notes
Ensure your AWS IAM user and ECR has correct permissions to push images.
Maintain security by storing sensitive keys in GitHub Secrets.
Consider tagging docker images using Git SHA or semantic versioning for better tracking.
Kubernetes and ArgoCD require additional cluster setup and configuration beyond the scope of traditional Docker Compose.

References and Resources
Todo-List-nodejs original repo
Dockerizing Node.js & MongoDB - Tutorials
GitHub Actions for Docker CI/CD
Ansible documentation
Kubernetes & ArgoCD Docs

