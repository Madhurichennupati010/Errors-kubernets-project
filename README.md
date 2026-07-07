# Kubernetes Troubleshooting Labs

## Project Overview

This repository contains practical Kubernetes troubleshooting scenarios that simulate real-world production issues.

The goal of this project is to improve debugging skills using Kubernetes CLI commands and understand the root cause of common pod failures.

---

## Scenarios Covered

- CrashLoopBackOff
- ErrImagePull
- Pending Pod (Insufficient Resources)

---

## Tools Used

- AWS EC2 (Ubuntu)
- Docker
- Minikube
- kubectl
- Kubernetes

---

## Troubleshooting Commands

```bash
kubectl get pods

kubectl describe pod <pod-name>

kubectl logs <pod-name>

kubectl logs --previous <pod-name>

kubectl get events

kubectl get nodes
```

---

## Repository Structure

```
01-crashloopbackoff/
02-errimagepull/
03-pending-pod/
```

---

## Learning Outcomes

- Debug Pod startup failures
- Investigate image pull issues
- Analyze scheduler events
- Read Kubernetes Events
- Understand Pod lifecycle
