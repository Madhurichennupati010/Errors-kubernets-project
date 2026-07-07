# Pending Pod (Insufficient Resources)

## Overview

This lab demonstrates how a Pod enters the **Pending** state when Kubernetes is unable to schedule it due to insufficient CPU or memory resources available on the cluster.

This is a common issue in production environments where the resource requests of a Pod exceed the capacity of the available worker nodes.

---

## Objective

- Understand why a Pod remains in the **Pending** state.
- Learn how Kubernetes schedules Pods.
- Practice troubleshooting using Kubernetes commands.
- Identify the root cause and resolve the issue.

---

## Prerequisites

- AWS EC2 (Ubuntu)
- Docker
- Minikube
- kubectl
- Kubernetes Cluster

---

## Pod YAML

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: pending-demo
spec:
  containers:
  - name: nginx
    image: nginx
    resources:
      requests:
        memory: "20Gi"
        cpu: "8"
```

---

## Deploy the Pod

```bash
kubectl apply -f pending-pod.yaml
```

Expected Output

```text
pod/pending-demo created
```

---

## Verify Pod Status

```bash
kubectl get pods
```

Expected Output

```text
NAME           READY   STATUS    RESTARTS   AGE
pending-demo   0/1     Pending   0          10s
```

---

## Troubleshooting Steps

### 1. Check Pod Status

```bash
kubectl get pods
```

---

### 2. Describe the Pod

```bash
kubectl describe pod pending-demo
```

Example Output

```text
Events:

Warning  FailedScheduling

0/1 nodes are available:

Insufficient cpu

Insufficient memory
```

---

### 3. Check Node Capacity

```bash
kubectl describe node minikube
```

This command displays:

- Total CPU
- Total Memory
- Allocatable Resources
- Currently Allocated Resources

---

### 4. Check Cluster Nodes

```bash
kubectl get nodes
```

Expected Output

```text
NAME        STATUS   ROLES           AGE
minikube    Ready    control-plane
```

---

## Root Cause

The Pod requests more CPU and memory than the Kubernetes node can provide.

Requested Resources:

- CPU : 8 Cores
- Memory : 20Gi

Available Resources:

- CPU : Less than requested
- Memory : Less than requested

Since no node satisfies the resource request, the scheduler cannot place the Pod on any node.

Therefore, the Pod remains in the **Pending** state.

---

## Resolution

Reduce the requested resources to values that fit within the node's available capacity.

Example:

```yaml
resources:
  requests:
    cpu: "100m"
    memory: "128Mi"
```

Delete the existing Pod:

```bash
kubectl delete pod pending-demo
```

Deploy the updated Pod:

```bash
kubectl apply -f pending-pod.yaml
```

Verify:

```bash
kubectl get pods
```

Expected Output

```text
NAME           READY   STATUS    RESTARTS   AGE
pending-demo   1/1     Running   0          15s
```

---

## Kubernetes Commands Used

```bash
kubectl apply -f pending-pod.yaml

kubectl get pods

kubectl describe pod pending-demo

kubectl describe node minikube

kubectl get nodes

kubectl delete pod pending-demo
```

---

## Key Learnings

- Understood how Kubernetes schedules Pods.
- Learned how CPU and memory requests affect scheduling.
- Identified the cause of a Pod remaining in the Pending state.
- Used `kubectl describe pod` to analyze scheduler events.
- Verified node capacity using `kubectl describe node`.
- Resolved the issue by reducing resource requests.

---

## Interview Questions

### Q1. What does the Pending status indicate?

**Answer:**

A Pod is in the **Pending** state when Kubernetes has accepted the Pod but has not yet been able to schedule it onto a node. Common reasons include insufficient CPU, insufficient memory, missing Persistent Volumes, or scheduling constraints.

---

### Q2. How do you troubleshoot a Pending Pod?

**Answer:**

1. Check the Pod status using:

```bash
kubectl get pods
```

2. Describe the Pod:

```bash
kubectl describe pod <pod-name>
```

3. Review the Events section for scheduler messages.

4. Check node resources:

```bash
kubectl describe node <node-name>
```

5. Reduce resource requests or increase cluster capacity.

---

### Project Outcome

This lab demonstrates practical experience in troubleshooting Kubernetes scheduling issues by identifying insufficient resource allocation and resolving the Pending Pod state.
