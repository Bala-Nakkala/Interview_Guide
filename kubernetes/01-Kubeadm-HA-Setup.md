# Kubernetes High Availability (HA) Setup using kubeadm

## Objective

Learn how to build a highly available Kubernetes cluster using **kubeadm**, **HAProxy**, **Virtual IP (VIP)**, and **3 Control Plane Nodes**.

---

# Architecture

```text
                    kubectl
                        |
                        |
                 Virtual IP (VIP)
                 192.168.1.100:6443
                        |
                    HAProxy
                        |
        ---------------------------------
        |               |               |
     Master1        Master2        Master3
        |               |               |
        +---------------+---------------+
                etcd Replication
                        |
         -------------------------------
         |                             |
      Worker1                      Worker2
```

---

# Components

| Component | Purpose |
|-----------|----------|
| kubeadm | Creates and manages Kubernetes clusters |
| kubelet | Runs on every node and manages Pods |
| kubectl | CLI tool to interact with Kubernetes |
| containerd | Container Runtime |
| HAProxy | Load balances API Server requests |
| Virtual IP (VIP) | Single endpoint for Kubernetes API |
| etcd | Stores Kubernetes cluster state |

---

# Step 1 - Prepare Linux Servers

Example Servers

```text
Master1
Master2
Master3
Worker1
Worker2
```

---

## Set Hostname

```bash
hostnamectl set-hostname master1
```

### Why?

Assigns a unique hostname to every server.

---

## Disable Swap

```bash
swapoff -a
```

### Why?

Kubernetes requires swap to be disabled because kubelet expects predictable memory management.

---

## Install Container Runtime

```bash
apt install containerd -y
```

### Why?

Containerd is responsible for running containers.

---

## Install Kubernetes Packages

```bash
apt install kubelet kubeadm kubectl -y
```

### Purpose

| Package | Use |
|---------|-----|
| kubelet | Runs Pods |
| kubeadm | Creates and joins clusters |
| kubectl | Kubernetes CLI |

---

# Step 2 - Configure HAProxy

Network Team provides a Virtual IP.

Example

```text
VIP = 192.168.1.100
```

HAProxy forwards Kubernetes API requests.

```text
VIP
 |
HAProxy
 |
+---------+---------+
|         |         |
M1        M2        M3
```

HAProxy continuously checks which API Server is healthy.

---

# Step 3 - Initialize First Control Plane

Run only on Master1

```bash
kubeadm init \
--control-plane-endpoint=192.168.1.100:6443 \
--upload-certs
```

---

## Command Explanation

### --control-plane-endpoint

```text
192.168.1.100:6443
```

This is the Virtual IP.

All nodes communicate with the VIP instead of a specific master node.

---

### --upload-certs

Uploads Kubernetes certificates securely.

These certificates are required when additional control plane nodes join the cluster.

---

# Step 4 - Configure kubectl

```bash
mkdir -p $HOME/.kube

cp -i /etc/kubernetes/admin.conf $HOME/.kube/config

chown $(id -u):$(id -g) $HOME/.kube/config
```

### Why?

Allows kubectl to communicate with the Kubernetes API Server.

---

# Step 5 - Join Additional Control Plane Nodes

Run on Master2 and Master3.

```bash
kubeadm join 192.168.1.100:6443 \
--token <token> \
--discovery-token-ca-cert-hash sha256:<hash> \
--control-plane \
--certificate-key <certificate-key>
```

---

## Command Explanation

### --token

Temporary authentication token.

Used to verify that the joining node is authorized.

Default validity: **24 Hours**

---

### --discovery-token-ca-cert-hash

Verifies the identity of the Kubernetes API Server.

Protects against Man-in-the-Middle (MITM) attacks.

---

### --control-plane

Joins the node as a Control Plane.

Without this option, the node becomes a Worker Node.

---

### --certificate-key

Downloads Kubernetes certificates securely from the first control plane.

Only required for additional Control Plane nodes.

---

# Step 6 - Join Worker Nodes

Run on Worker1 and Worker2.

```bash
kubeadm join 192.168.1.100:6443 \
--token <token> \
--discovery-token-ca-cert-hash sha256:<hash>
```

Notice

Worker nodes do NOT require

- --control-plane
- --certificate-key

---

# Request Flow

```text
kubectl apply deployment.yaml
            |
            |
          VIP
            |
         HAProxy
            |
Healthy Control Plane
            |
       API Server
            |
       Scheduler
            |
      Worker Node
            |
         Pod Runs
```

---

# What happens if Master1 fails?

1. HAProxy detects Master1 is unhealthy.
2. HAProxy forwards new requests to Master2.
3. Master2 already has the latest cluster state.
4. Existing Pods continue running on Worker Nodes.

No application downtime.

---

# etcd Replication

Every Control Plane contains an etcd member.

The following Kubernetes objects are replicated continuously.

- Deployments
- Services
- Secrets
- ConfigMaps
- Nodes
- Namespaces
- ReplicaSets

> Note:
> etcd stores the desired state of Pods, not the running containers themselves.

---

# Quorum Formula

```text
N / 2 + 1
```

Examples

| Control Planes | Minimum Required |
|---------------|------------------|
| 3 | 2 |
| 5 | 3 |
| 7 | 4 |

Without quorum, Kubernetes blocks write operations.

---

# Responsibilities

## Control Plane

- API Server
- Scheduler
- Controller Manager
- etcd

## Worker Node

- Runs Pods
- Runs Containers
- Executes Applications

---

# On-Prem vs AWS EKS

| On-Prem | AWS EKS |
|----------|----------|
| We install kubeadm | AWS manages control plane |
| We manage etcd | AWS manages etcd |
| We configure HAProxy | No HAProxy required |
| We maintain HA | AWS provides HA |

---

# Interview Questions

### Why do we need HAProxy?

HAProxy distributes API requests to healthy control plane nodes and prevents a single point of failure.

---

### Why do we use a Virtual IP?

A Virtual IP provides a single endpoint for kubectl, worker nodes, and administrators.

---

### Why do we use three control plane nodes?

To satisfy the etcd quorum rule (N/2 + 1) and ensure High Availability.

---

### Where do Pods run?

Pods run only on Worker Nodes.

---

### What does etcd store?

- Deployments
- Services
- Secrets
- ConfigMaps
- Nodes
- Namespaces
- Cluster State

---

# Summary

- Build the first control plane using `kubeadm init`.
- Join additional control planes using `kubeadm join --control-plane`.
- Join worker nodes using `kubeadm join`.
- All nodes communicate through the Virtual IP.
- HAProxy forwards requests to healthy API Servers.
- etcd continuously replicates the cluster state.
- Worker nodes run Pods.
- Three control planes provide High Availability using quorum.