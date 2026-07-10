# 01 - KIND Cluster Setup

## 🎯 Goal

By the end of this guide, you'll have a fully functional local Kubernetes cluster using KIND (Kubernetes IN Docker) and understand the purpose of each component involved in the setup.

---

# 🤔 Why KIND?

Before deploying applications on Kubernetes, we need a Kubernetes cluster.

There are several options available:

| Tool | Best For |
|-------|----------|
| KIND | Local development and learning |
| Minikube | Local development |
| k3d | Lightweight Kubernetes |
| Amazon EKS | Production |
| Azure AKS | Production |
| Google GKE | Production |

For this repository, we'll use **KIND** because it is lightweight, fast, easy to set up, and runs Kubernetes clusters inside Docker containers.

---

# 📋 Prerequisites

Before you begin, make sure the following tools are installed.

- Docker Desktop
- kubectl
- KIND
- Git
- Visual Studio Code

Verify the installation.

```bash
docker --version
kubectl version --client
kind version
git --version
```

---

# 🏗️ Cluster Architecture

Our cluster consists of:

- 1 Control Plane Node
- 2 Worker Nodes

```
                Kubernetes Cluster

               ┌────────────────────┐
               │   Control Plane    │
               └─────────┬──────────┘
                         │
          ┌──────────────┴──────────────┐
          │                             │
   ┌───────────────┐             ┌───────────────┐
   │  Worker Node  │             │  Worker Node  │
   └───────────────┘             └───────────────┘
```

The Control Plane manages the cluster, while the Worker Nodes run your applications.

---

# 📄 Step 1 - Create the Cluster Configuration

Create a file named:

```
kind-config.yaml
```

Paste the following configuration.

```yaml
kind: Cluster
apiVersion: kind.x-k8s.io/v1alpha4

nodes:
- role: control-plane
  image: kindest/node:v1.35.1

- role: worker
  image: kindest/node:v1.35.1

- role: worker
  image: kindest/node:v1.35.1
```

---

# 🚀 Step 2 - Create the Cluster

Run the following command.

```bash
kind create cluster --config kind-config.yaml --name my-kind-cluster
```

This creates a Kubernetes cluster with one control-plane node and two worker nodes.

---

# ✅ Step 3 - Verify the Cluster

Check whether the cluster has been created successfully.

```bash
kubectl get nodes
```

Expected Output

```
NAME                         STATUS   ROLES           AGE
my-kind-cluster-control-plane Ready   control-plane
my-kind-cluster-worker        Ready
my-kind-cluster-worker2       Ready
```

View cluster information.

```bash
kubectl cluster-info
```

View all system Pods.

```bash
kubectl get pods -A
```

---

# 🔍 Understanding the Output

## kubectl get nodes

Displays all nodes available in the cluster.

## kubectl cluster-info

Shows the Kubernetes API Server endpoint.

## kubectl get pods -A

Lists every Pod running across all namespaces.

You'll notice several Pods that Kubernetes creates automatically, such as CoreDNS and kube-proxy.

---

# 🧪 Hands-on Lab

Try the following commands.

```bash
kubectl get nodes

kubectl get namespaces

kubectl get pods -A

kubectl get all -A
```

Observe how Kubernetes organizes its resources.

---

# 🛠️ Common Errors

## kind: command not found

Cause

KIND is not installed or not added to PATH.

Solution

Verify the installation.

```bash
kind version
```

---

## kubectl: command not found

Cause

kubectl is not installed.

Solution

Install kubectl and verify.

```bash
kubectl version --client
```

---

## Cannot connect to the Kubernetes cluster

Cause

The cluster is not running.

Solution

List all clusters.

```bash
kind get clusters
```

Create the cluster if it doesn't exist.

```bash
kind create cluster --name my-kind-cluster
```

---

## Docker is not running

Cause

Docker Desktop is stopped.

Solution

Start Docker Desktop before creating the cluster.

---

# ✅ Production Checklist

- Install the latest stable version of KIND.
- Keep kubectl and Kubernetes versions compatible.
- Use a custom cluster name.
- Store cluster configuration files in version control.
- Verify the cluster after creation.
- Delete unused clusters to free system resources.

---

# 🎤 Interview Questions

### What is KIND?

### Why do we use KIND instead of Minikube?

### What is the difference between the Control Plane and Worker Nodes?

### Can we create multiple KIND clusters?

### Where does KIND run the Kubernetes cluster?

### How do you verify that a Kubernetes cluster is healthy?

---

# 🧹 Clean Up

Delete the cluster.

```bash
kind delete cluster --name my-kind-cluster
```

Verify.

```bash
kind get clusters
```

---

# 📌 Summary

In this chapter, you learned how to:

- Install the required tools.
- Create a local Kubernetes cluster using KIND.
- Verify the cluster.
- Understand the role of Control Plane and Worker Nodes.
- Troubleshoot common setup issues.
- Remove the cluster when it's no longer needed.

Your Kubernetes environment is now ready for deploying applications.

💼 Real-World Scenario

You're joining a new company.

The team asks you to create a local Kubernetes cluster that mirrors production as closely as possible for testing a new application.

Which tool would you choose and why?

Would you use KIND, Minikube, k3d, or a cloud-managed Kubernetes service?

Think about the trade-offs before reading the next chapter.
