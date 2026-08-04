## Jenkins on Kubernetes Architecture

```text
                                 Developer
                                     │
                               Git Push Code
                                     │
                                     ▼
                   GitHub (Application Repository)
                                     │
                              GitHub Webhook
                                     │
                                     ▼
                          Jenkins Controller
                         (Always Running)
                                     │
                       Reads Jenkinsfile
                                     │
                                     ▼
                         Kubernetes Plugin
                                     │
                       Requests Kubernetes API
                                     │
                                     ▼
                              Kubernetes API
                                     │
      -----------------------------------------------------------------
      │                               │                              │
      ▼                               ▼                              ▼
  Dynamic Agent Pod              Dynamic Agent Pod             Dynamic Agent Pod
      (Build #1)                    (Build #2)                   (Build #3)
      │                               │                              │
      ├── JNLP                         ├── JNLP                      ├── JNLP
      ├── Maven                        ├── Maven                     ├── Maven
      ├── Trivy                        ├── Trivy                     ├── Trivy
      └── Kaniko                       └── Kaniko                    └── Kaniko
      │                               │                              │
      ▼                               ▼                              ▼
  Build Application             Build Application             Build Application
      │                               │                              │
      ▼                               ▼                              ▼
 Build Docker Image            Build Docker Image            Build Docker Image
      │                               │                              │
      ▼                               ▼                              ▼
 Push Image to ECR/DockerHub    Push Image to ECR/DockerHub   Push Image to ECR/DockerHub
             │
             ▼
      Update Image Tag
             │
             ▼
      GitHub (GitOps Repository)
             │
             ▼
           Argo CD
             │
             ▼
      Kubernetes Cluster
             │
             ▼
  Rolling Update / Sync Application
             │
             ▼
   Agent Pod Automatically Deleted
             │
             ▼
      Jenkins Controller Remains Running
```