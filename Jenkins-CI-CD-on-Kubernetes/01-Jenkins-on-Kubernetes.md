# Jenkins on Kubernetes - Interview Notes

## Why Jenkins on Kubernetes?

Traditional Jenkins executes all builds inside the Jenkins server itself. As the number of builds increases, the Jenkins server consumes high CPU, RAM, and storage, making it difficult to scale.

Jenkins on Kubernetes solves this problem by creating a temporary Kubernetes Agent Pod for every build. The build runs inside the Agent Pod, and after the pipeline finishes, the Pod is automatically deleted. The Jenkins Controller only manages the pipeline and UI.

---

# Traditional Jenkins vs Jenkins on Kubernetes

## Traditional Jenkins

- One Jenkins VM
- All builds run inside the Jenkins server
- High CPU and RAM usage
- Difficult to scale
- Multiple builds slow down the server

## Jenkins on Kubernetes

- Jenkins Controller manages pipelines only
- Every build runs in a temporary Agent Pod
- Pods are deleted after the build
- Easy to scale
- Multiple builds can run in parallel
- Better resource utilization

---

# Components Used

## 1. Jenkins Controller

Purpose:
- Provides Jenkins UI
- Stores pipeline jobs
- Receives GitHub Webhooks
- Creates Agent Pods
- Shows build logs

Always Running: **Yes**

---

## 2. Kubernetes Plugin

Purpose:
- Connects Jenkins with Kubernetes
- Creates Dynamic Agent Pods
- Deletes Agent Pods after build

Simple Definition:
**Bridge between Jenkins and Kubernetes.**

---

## 3. pod-templates.yaml

Purpose:
Defines how the Agent Pod should be created.

Contains:
- JNLP Container
- Maven Container
- Kaniko Container
- Trivy Container
- Shared Workspace
- Volumes

Simple Definition:
**Blueprint of the Jenkins Agent Pod.**

---

## 4. Jenkins-controller.yaml

Purpose:
Creates the Jenkins Controller.

Contains:
- Deployment
- Service
- PVC
- Resources
- Environment Variables

Simple Definition:
**Creates the Jenkins Server on Kubernetes.**

---

## 5. JNLP

Full Form:
Java Network Launch Protocol

Purpose:
Connects the Agent Pod with the Jenkins Controller.

Simple Definition:
**Communication channel between Jenkins Controller and Agent Pod.**

---

## 6. Maven

Purpose:
- Download Dependencies
- Compile Code
- Run Tests
- Build JAR

Output:
app.jar

---

## 7. Kaniko

Purpose:
Builds Docker Images inside Kubernetes without using Docker Daemon.

Tasks:
- Read Dockerfile
- Build Image
- Push Image to DockerHub

---

## 8. Trivy

Purpose:
Scans Docker Images for vulnerabilities.

Checks:
- Critical
- High
- Medium
- Low

---

## 9. Jenkinsfile

Purpose:
Defines the CI Pipeline.

Example Stages:

- Checkout
- Build
- Test
- Scan
- Build Image
- Push Image
- Update Helm Values

---

## 10. Dockerfile

Purpose:
Defines how the Docker Image should be built.

---

# Why Two GitHub Repositories?

## Repository 1 - Application Repository

Contains:
- Source Code
- Jenkinsfile
- Dockerfile

Purpose:
Developers push application code here.

---

## Repository 2 - GitOps Repository

Contains:
- Helm Charts
- Kubernetes Manifests
- values.yaml

Purpose:
Jenkins updates only the image tag.
Argo CD watches this repository and deploys the latest version.

---

# Why Not One Repository?

Separating application code and deployment configuration follows GitOps best practices.

Benefits:
- Better security
- Easier rollback
- Clean version control
- CI and CD are separated
