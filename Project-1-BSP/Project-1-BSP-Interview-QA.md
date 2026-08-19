# Project 1 – Biometric Service Provider (BSP)
## Project-Based Interview Questions & Answers

**Role:** DevOps Engineer  
**Environment:** AWS EKS, Jenkins, Argo CD, Terraform, Ansible, Docker, Helm, Kafka, ZooKeeper, RabbitMQ, Prometheus, Grafana, ELK Stack

---

## 1. Give me an overview of Project 1.

**Answer:**

> "Project 1 was a Biometric Service Provider platform where we deployed and supported containerized microservices on AWS EKS. My role was mainly focused on Kubernetes, CI/CD, GitOps, AWS infrastructure, automation, messaging, monitoring, and logging. We used Docker and Helm for application deployment, Jenkins for CI/CD, Argo CD for GitOps-based deployments, Terraform and Ansible for automation, Kafka and ZooKeeper for streaming, RabbitMQ for messaging, and Prometheus, Grafana, and ELK Stack for observability."

---

## 2. What was your role in the project?

**Answer:**

> "I worked as a DevOps Engineer. My responsibilities included supporting AWS infrastructure, managing Kubernetes workloads on EKS, maintaining Jenkins CI/CD pipelines, supporting Argo CD deployments, creating Terraform configurations and Ansible automation, supporting Kafka and ZooKeeper, and monitoring applications and infrastructure using Prometheus, Grafana, and ELK."

---

# Application & Kubernetes

## 3. How did you deploy the microservices?

**Answer:**

> "We containerized the microservices using Docker and deployed them on AWS EKS. Kubernetes handled scheduling, scaling, service discovery, and application availability. Helm was used to package and manage Kubernetes application deployments."

## 4. Why did you use AWS EKS?

**Answer:**

> "EKS provides a managed Kubernetes control plane, so we could focus more on application workloads and DevOps operations instead of manually managing Kubernetes control-plane components. It also integrates well with AWS services."

## 5. Why Kubernetes for microservices?

**Answer:**

> "Microservices require reliable deployment, scaling, service discovery, self-healing, and container orchestration. Kubernetes provides these capabilities through Deployments, Services, ReplicaSets, health checks, and controllers."

## 6. What is Helm and why did you use it?

**Answer:**

> "Helm is a package manager for Kubernetes. Instead of maintaining many Kubernetes YAML files independently, we can create a reusable Helm chart with templates and values. This makes deployments consistent and easier to manage across environments."

---

# CI/CD

## 7. Explain your Jenkins CI/CD pipeline.

**Answer:**

> "A developer commits code to Git. Jenkins triggers the pipeline and performs the required build and validation steps. The application is packaged into a Docker image, and the image is published to the container image repository. The deployment side is handled through our GitOps process using Argo CD."

## 8. What was Jenkins responsible for?

**Answer:**

> "Jenkins was responsible for automating the CI/CD process. It executed build, test, packaging, container image creation, and deployment-related steps."

## 9. Why did you use Argo CD along with Jenkins?

**Answer:**

> "We separated CI from CD. Jenkins handled the CI activities such as building and packaging the application. Argo CD handled Kubernetes deployment using GitOps. Argo CD continuously monitored the desired configuration in Git and synchronized it with the Kubernetes cluster."

## 10. What is GitOps?

**Answer:**

> "GitOps means Git is treated as the source of truth for the desired application and infrastructure configuration. Instead of manually changing Kubernetes resources, we update the configuration in Git and a GitOps tool such as Argo CD synchronizes the cluster."

## 11. What is the advantage of this approach?

**Answer:**

> "It provides version control, auditability, rollback capability, and a clear history of configuration changes."

---

# AWS

## 12. Which AWS services did you work with?

**Answer:**

> "I worked with EC2, ECR, RDS, S3, VPC, IAM, and CloudWatch."

## 13. What is EC2?

**Answer:**

> "EC2 provides virtual servers in AWS. It can be used to run applications, infrastructure components, or supporting services."

## 14. What is ECR?

**Answer:**

> "Amazon ECR is a managed container image registry. Docker images can be pushed to ECR and later pulled by Kubernetes workloads running on AWS."

## 15. What is RDS?

**Answer:**

> "Amazon RDS is a managed relational database service. AWS handles many database administration tasks such as backups, patching, and infrastructure management."

## 16. What is S3?

**Answer:**

> "Amazon S3 is an object storage service. It is commonly used for backups, files, artifacts, logs, and other objects."

## 17. What is VPC?

**Answer:**

> "A VPC is an isolated virtual network in AWS. It contains resources such as subnets, route tables, security groups, and other networking components."

## 18. What is IAM?

**Answer:**

> "IAM controls authentication and authorization in AWS. We use users, roles, and policies to control access to AWS resources."

## 19. What is CloudWatch?

**Answer:**

> "CloudWatch is AWS's monitoring and observability service. It provides metrics, logs, alarms, and other monitoring capabilities for AWS resources and applications."

---

# Infrastructure Automation

## 20. Why did you use Terraform?

**Answer:**

> "Terraform was used for Infrastructure as Code. Instead of manually creating AWS resources, we could define the infrastructure in configuration files and provision it consistently."

## 21. What is the advantage of Terraform?

**Answer:**

> "Terraform provides repeatability, version control, automation, and predictable infrastructure changes."

## 22. Why did you use Ansible?

**Answer:**

> "Ansible was used for configuration management and server automation. It helped automate repetitive tasks across servers using playbooks, roles, and YAML."

## 23. Terraform vs Ansible?

**Answer:**

> "Terraform is mainly used to provision infrastructure, while Ansible is mainly used to configure and manage systems after they are provisioned."

---

# Messaging & Streaming

## 24. Why did you use Kafka?

**Answer:**

> "Kafka was used for high-throughput event streaming. It allows producers to publish events and consumers to process those events asynchronously."

## 25. What is ZooKeeper's role with Kafka?

**Answer:**

> "In the Kafka architecture used in the project, ZooKeeper was responsible for maintaining cluster metadata and coordination between Kafka brokers."

> **Interview note:** Modern Kafka can run without ZooKeeper using KRaft mode. If the interviewer asks about current Kafka architecture, mention that newer Kafka deployments can use KRaft.

## 26. What is RabbitMQ?

**Answer:**

> "RabbitMQ is a message broker used for asynchronous communication between applications. It supports queues, exchanges, routing, acknowledgements, and reliable message delivery."

## 27. Kafka vs RabbitMQ?

**Answer:**

> "Kafka is primarily designed for high-throughput event streaming and durable event logs. RabbitMQ is primarily a message broker focused on message routing and queue-based communication."

---

# Automation & Infrastructure Support

## 28. How did you handle infrastructure automation?

**Answer:**

> "We used Terraform for infrastructure provisioning and Ansible playbooks and roles for configuration management. This reduced manual work and made environment setup more repeatable."

## 29. What is Infrastructure as Code?

**Answer:**

> "Infrastructure as Code means defining infrastructure using code or configuration files instead of creating resources manually. Terraform is a common example."

---

# Backup & Recovery

## 30. What backup and recovery activities did you perform?

**Answer:**

> "We implemented backup and recovery procedures for critical systems and databases. The objective was to protect important data and provide a recovery procedure in case of failures."

## 31. Why is backup and recovery important?

**Answer:**

> "High availability protects against many runtime failures, but it does not replace backups. Backups provide a recovery point when data is accidentally deleted, corrupted, or affected by a major failure."

---

# Monitoring & Observability

## 32. How did you monitor the platform?

**Answer:**

> "We used Prometheus for collecting metrics and Grafana for visualization. We also used the ELK Stack for centralized logging."

## 33. What is Prometheus?

**Answer:**

> "Prometheus is a monitoring and metrics collection system. It collects time-series metrics and allows us to query them using PromQL."

## 34. What is Grafana?

**Answer:**

> "Grafana is a visualization and dashboarding tool. We used it to create dashboards from Prometheus metrics and identify application or infrastructure issues."

## 35. Why do you need both Prometheus and Grafana?

**Answer:**

> "Prometheus collects and stores metrics, while Grafana visualizes those metrics through dashboards. They complement each other."

## 36. What is ELK Stack?

**Answer:**

> "ELK stands for Elasticsearch, Logstash, and Kibana. Elasticsearch stores and searches logs, Logstash processes and forwards logs, and Kibana provides visualization and search."

## 37. Why use centralized logging?

**Answer:**

> "Instead of checking logs on individual servers or pods, centralized logging allows us to search and analyze logs from one place. This makes troubleshooting much faster."

---

# Scenario-Based Questions

## 38. A microservice is down in EKS. How would you troubleshoot it?

**Answer:**

> "First, I would check the pod status using `kubectl get pods`. Then I would check pod events and logs using `kubectl describe pod` and `kubectl logs`. I would verify the Deployment and ReplicaSet, check the Service and endpoints, and then check resource usage and application health. If required, I would also check Prometheus/Grafana metrics and centralized logs."

## 39. A deployment is failing. What would you check?

**Answer:**

> "I would check the Jenkins pipeline logs first to identify whether the issue is in the build or image stage. If CI is successful, I would check the container image, Kubernetes manifests or Helm values, pod events, application logs, and Argo CD synchronization status."

## 40. Application response time suddenly increases. What would you check?

**Answer:**

> "I would check CPU and memory metrics, pod health, request rates, application logs, database performance, and other dependent services. Prometheus and Grafana would help identify metric-related bottlenecks, while ELK would help investigate application errors."

---

# Project 1 – Short Interview Summary

> "In Project 1, I worked on a biometric microservices platform running on AWS EKS. My main responsibilities were Kubernetes operations, Jenkins CI/CD, Argo CD GitOps deployments, AWS infrastructure, Terraform and Ansible automation, Kafka and ZooKeeper support, RabbitMQ messaging, and observability using Prometheus, Grafana, and ELK. I also supported backup and recovery procedures for critical systems and databases."

---

# Key Technologies to Remember

| Technology | Purpose |
|---|---|
| AWS EKS | Managed Kubernetes |
| EC2 | Virtual compute |
| ECR | Container image registry |
| RDS | Managed relational database |
| S3 | Object storage |
| VPC | AWS networking |
| IAM | Access control |
| CloudWatch | AWS monitoring |
| Kubernetes | Container orchestration |
| Docker | Containerization |
| Helm | Kubernetes package management |
| Jenkins | CI/CD automation |
| Argo CD | GitOps continuous delivery |
| Terraform | Infrastructure provisioning |
| Ansible | Configuration management |
| Kafka | Event streaming |
| ZooKeeper | Kafka coordination in the project architecture |
| RabbitMQ | Message broker |
| Prometheus | Metrics collection |
| Grafana | Metrics visualization |
| ELK | Centralized logging |
