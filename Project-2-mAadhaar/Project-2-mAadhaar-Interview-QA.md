# Project 2 – mAadhaar – UIDAI Mobile Application
## Project-Based Interview Questions & Answers

**Role:** DevOps Engineer  
**Infrastructure:** OpenStack Private Cloud  
**Client:** UIDAI (Government of India)  
**Environment:** Kubernetes, kubeadm, Jenkins, Dynamic Kubernetes Agents, Kaniko, JFrog Artifactory, Argo CD, Argo Rollouts, Terraform, Ansible, NGINX Ingress, Prometheus, Grafana, ELK

---

## 1. Give me an overview of Project 2.

**Answer:**

> "Project 2 was the mAadhaar mobile application platform for UIDAI. The application ran on an OpenStack private cloud environment. My main responsibilities included designing and managing highly available Kubernetes clusters using kubeadm, building Jenkins CI/CD pipelines with dynamic Kubernetes agents and Kaniko, implementing Canary deployments using Argo CD and Argo Rollouts, automating configuration with Ansible, monitoring with Prometheus and Grafana, analyzing logs using the ELK Stack, and configuring NGINX Ingress with TLS termination."

---

## 2. What was your role in Project 2?

**Answer:**

> "I worked as a DevOps Engineer responsible for Kubernetes platform operations, CI/CD automation, deployment strategies, server configuration automation, monitoring, logging, and application traffic management."

---

# OpenStack & Kubernetes

## 3. Why did you use OpenStack?

**Answer:**

> "The application infrastructure was running on a private cloud. OpenStack provided the compute and infrastructure platform on which we created and managed the required virtual machines and Kubernetes infrastructure."

## 4. What is OpenStack?

**Answer:**

> "OpenStack is an open-source cloud platform used to provide private-cloud infrastructure services such as compute, networking, storage, and identity."

## 5. How did you set up Kubernetes?

**Answer:**

> "We used kubeadm to bootstrap the Kubernetes cluster. The cluster was designed with three control-plane nodes for high availability and multiple worker nodes for running application workloads."

## 6. Why three control-plane nodes?

**Answer:**

> "Three control-plane nodes provide high availability and allow the cluster to tolerate a control-plane node failure while maintaining quorum for the Kubernetes control-plane data."

## 7. What is kubeadm?

**Answer:**

> "kubeadm is a Kubernetes tool used to bootstrap and configure a Kubernetes cluster. It helps initialize control-plane nodes and join additional control-plane or worker nodes."

## 8. What is a control-plane node?

**Answer:**

> "The control plane manages the Kubernetes cluster. Major components include the API server, scheduler, controller manager, and the cluster's state store."

## 9. What is a worker node?

**Answer:**

> "A worker node runs application workloads. It normally contains components such as kubelet, a container runtime, and kube-proxy."

## 10. Why is Kubernetes HA important?

**Answer:**

> "High availability reduces the impact of node failures. If one control-plane node becomes unavailable, the remaining control-plane nodes can continue serving the cluster, provided quorum and the overall cluster design are maintained."

---

# Jenkins CI/CD on Kubernetes

## 11. Why did you run Jenkins on Kubernetes?

**Answer:**

> "We used Kubernetes to provide dynamic build agents. Instead of maintaining a fixed Jenkins worker, Jenkins could create an agent pod for each build. After the build finishes, the agent pod is removed. This provides better resource utilization and allows multiple builds to run in isolated environments."

## 12. What is a Jenkins Controller?

**Answer:**

> "The Jenkins Controller is the main Jenkins component. It manages jobs, pipelines, credentials, configuration, scheduling, and the Jenkins UI. In our setup, it remains available while temporary agent pods execute the actual build workload."

## 13. What is a Dynamic Kubernetes Agent?

**Answer:**

> "It is a temporary Jenkins worker pod created on demand inside Kubernetes. Jenkins uses the Kubernetes plugin to request the pod, execute the pipeline stages, and remove the pod after the build completes."

## 14. What is the Kubernetes Plugin in Jenkins?

**Answer:**

> "The Jenkins Kubernetes plugin allows Jenkins to dynamically create and manage build agent pods in a Kubernetes cluster."

## 15. What is JNLP in Jenkins?

**Answer:**

> "JNLP is the communication mechanism traditionally used by Jenkins agents to connect back to the Jenkins Controller. In a Kubernetes agent pod, the Jenkins agent container establishes the connection so the controller can send pipeline work to the agent."

## 16. What is pod-template.yaml?

**Answer:**

> "The pod template defines how the Jenkins dynamic agent pod should be created. It specifies the containers, images, resources, workspace-related configuration, and tools required by the pipeline."

## 17. Why do you need multiple containers in an agent pod?

**Answer:**

> "Different containers can provide different tools while sharing the Jenkins workspace. For example, one container can provide Maven, another can provide Kaniko, and another can provide Trivy."

---

# Build Tools

## 18. What is Maven?

**Answer:**

> "Maven is a Java build and dependency-management tool. In the pipeline, it is used to compile the application, resolve dependencies, run tests, and package the application."

## 19. What is Kaniko?

**Answer:**

> "Kaniko builds container images from a Dockerfile without requiring a Docker daemon. This is useful in Kubernetes-based CI environments because we can build images without using privileged Docker-in-Docker."

## 20. Why Kaniko instead of Docker-in-Docker?

**Answer:**

> "Kaniko can build container images without a Docker daemon and is suitable for Kubernetes CI environments. It also avoids some of the security and operational concerns associated with running a privileged Docker daemon inside build containers."

## 21. What is Trivy?

**Answer:**

> "Trivy is a vulnerability scanner. We use it to scan container images and identify known vulnerabilities in operating-system packages and application dependencies."

## 22. What is JFrog Artifactory?

**Answer:**

> "JFrog Artifactory is an artifact and package repository. In our CI/CD process, it is used for centralized artifact and container image management."

---

# CI/CD Workflow

## 23. Explain your Jenkins pipeline end to end.

**Answer:**

> "A developer pushes application code to GitHub. A webhook triggers Jenkins. Jenkins reads the Jenkinsfile and requests a dynamic Kubernetes agent pod through the Kubernetes plugin. The agent contains the required build tools. Maven builds the application, Trivy performs vulnerability scanning, and Kaniko builds the container image. The image is pushed to the configured artifact repository. After the CI process completes, the deployment configuration is updated in Git, and Argo CD detects the change and synchronizes the application to Kubernetes."

## 24. What happens to the agent pod after the build?

**Answer:**

> "The dynamic agent pod is temporary. After the pipeline completes, Jenkins releases the agent and Kubernetes removes the pod. The Jenkins Controller remains available for future builds."

---

# Argo CD & Canary Deployment

## 25. Why did you use Argo CD?

**Answer:**

> "Argo CD provides GitOps-based continuous delivery for Kubernetes. It continuously compares the desired state stored in Git with the actual state in Kubernetes and synchronizes the cluster when changes are detected."

## 26. What is Argo Rollouts?

**Answer:**

> "Argo Rollouts is a Kubernetes controller that provides advanced deployment strategies such as Canary and Blue-Green deployments. It allows us to gradually release a new application version and control the rollout."

## 27. What is a Canary deployment?

**Answer:**

> "A Canary deployment gradually sends traffic to a new version while the majority of traffic continues to use the stable version. We monitor the new version and increase traffic gradually if it is healthy."

## 28. Why did you use Canary deployment?

**Answer:**

> "It reduces deployment risk. Instead of sending 100% of production traffic to a new version immediately, we expose the new version to a smaller percentage first. If problems are detected, we can stop or roll back the rollout."

## 29. What is the difference between Argo CD and Argo Rollouts?

**Answer:**

> "Argo CD is responsible for GitOps-based application delivery and synchronization. Argo Rollouts is responsible for advanced rollout strategies such as Canary and Blue-Green deployments."

---

# Ansible

## 30. Why did you use Ansible?

**Answer:**

> "We used Ansible for configuration management and server automation. It allowed us to standardize configuration across more than 250 Linux servers using reusable playbooks and roles."

## 31. What are Ansible playbooks and roles?

**Answer:**

> "A playbook defines the automation tasks to execute. A role provides a reusable structure for organizing variables, tasks, handlers, templates, and other configuration."

## 32. Why automate 250+ servers?

**Answer:**

> "Manual configuration does not scale well across hundreds of servers. Ansible makes configuration consistent, repeatable, and faster while reducing human error."

---

# Monitoring & Logging

## 33. How did you monitor the application?

**Answer:**

> "We used Prometheus to collect metrics and Grafana to visualize them. We also used the ELK Stack for centralized application and infrastructure logging."

## 34. What metrics would you monitor?

**Answer:**

> "I would monitor CPU, memory, pod restarts, pod availability, node health, application response time, request rate, error rate, and other application-specific metrics."

## 35. How did logging work?

**Answer:**

> "Application and infrastructure logs were collected and centralized using the ELK Stack. This allowed us to search and analyze logs from a central location instead of checking individual servers or pods."

---

# NGINX Ingress & TLS

## 36. Why did you use NGINX Ingress Controller?

**Answer:**

> "NGINX Ingress Controller provides HTTP and HTTPS routing into Kubernetes services. It allows us to expose multiple applications using host or path-based routing."

## 37. What is TLS termination?

**Answer:**

> "TLS termination means the incoming HTTPS connection is decrypted at the ingress layer. The NGINX Ingress Controller handles the TLS certificate and decrypts the request before forwarding traffic to the backend service according to the configured architecture."

## 38. Why is TLS important?

**Answer:**

> "TLS encrypts traffic between the client and the endpoint, protects sensitive information, and provides server identity through certificates."

---

# Scenario-Based Questions

## 39. One control-plane node goes down. What happens?

**Answer:**

> "Because the cluster has three control-plane nodes, the remaining control-plane nodes can continue serving the cluster if quorum is maintained. I would check node status, cluster health, API-server availability, and the affected node's infrastructure."

## 40. A Jenkins build is waiting for an agent. What would you check?

**Answer:**

> "I would check Jenkins queue status, the Kubernetes plugin configuration, Kubernetes API connectivity, pod-template configuration, namespace permissions, resource availability, and whether the agent pod is being created successfully."

## 41. Kaniko build fails. What would you check?

**Answer:**

> "I would check the Jenkins pipeline logs, Dockerfile, build context, Kaniko configuration, registry credentials, network connectivity, and the destination image repository."

## 42. A Canary deployment is failing. What would you check?

**Answer:**

> "I would check Argo Rollouts status, pod health, application logs, metrics, traffic routing, and the configured analysis or health checks. If the new version is unhealthy, I would stop or roll back the rollout."

## 43. Application is accessible internally but not externally. What would you check?

**Answer:**

> "I would check the application Service, Ingress resource, NGINX Ingress Controller, DNS, load-balancer or external networking configuration, TLS certificate, and firewall or security rules."

## 44. A pod is running but the application is not responding. What would you check?

**Answer:**

> "I would check container logs, application health endpoints, container ports, Service configuration, readiness probes, network connectivity, and resource usage."

---

# Impact Questions

## 45. What was the impact of your work?

**Answer:**

> "The environment supported a large private-cloud infrastructure. Through dynamic Jenkins Kubernetes agents, build queues were eliminated and CI resources were used on demand. We also reduced OpenStack idle compute overhead by approximately 60%."

## 46. How did dynamic Jenkins agents help reduce resource usage?

**Answer:**

> "Instead of keeping multiple dedicated Jenkins workers running continuously, agent pods were created only when builds required them. After completion, those pods were removed, which reduced idle compute consumption."

---

# Project 2 – Short Interview Summary

> "In Project 2, I worked on the mAadhaar platform running on OpenStack private cloud infrastructure. I designed and managed highly available Kubernetes clusters using kubeadm with three control-plane nodes and multiple workers. I built Jenkins CI/CD pipelines using dynamic Kubernetes agents and Kaniko, implemented Canary deployments using Argo CD and Argo Rollouts, automated configuration across 250+ Linux servers using Ansible, monitored the platform using Prometheus and Grafana, centralized logs using ELK, and configured NGINX Ingress with TLS termination."

---

# Key Technologies to Remember

| Technology | Purpose |
|---|---|
| OpenStack | Private cloud infrastructure |
| kubeadm | Kubernetes cluster bootstrap |
| Kubernetes | Container orchestration |
| Jenkins | CI/CD automation |
| Kubernetes Plugin | Creates dynamic Jenkins agent pods |
| JNLP | Jenkins agent-to-controller communication |
| Pod Template | Defines dynamic agent pod |
| Maven | Java build and dependency management |
| Kaniko | Container image building without Docker daemon |
| Trivy | Vulnerability scanning |
| JFrog Artifactory | Artifact/image repository |
| Argo CD | GitOps continuous delivery |
| Argo Rollouts | Canary/Blue-Green deployments |
| Ansible | Configuration management |
| NGINX Ingress | Kubernetes HTTP/HTTPS routing |
| TLS | Secure encrypted traffic |
| Prometheus | Metrics collection |
| Grafana | Metrics visualization |
| ELK | Centralized logging |

---

# Final Project 2 Interview Flow

```text
Developer
    │
    ▼
GitHub
    │
    ▼
Jenkins Webhook
    │
    ▼
Jenkins Controller
    │
    ▼
Kubernetes Plugin
    │
    ▼
Dynamic Agent Pod
    │
    ├── Maven
    ├── Trivy
    └── Kaniko
    │
    ▼
Build + Scan + Image
    │
    ▼
JFrog Artifactory
    │
    ▼
GitOps Repository
    │
    ▼
Argo CD
    │
    ▼
Argo Rollouts
    │
    ▼
Canary Deployment
    │
    ▼
Kubernetes on OpenStack
    │
    ├── Prometheus
    ├── Grafana
    └── ELK
```
