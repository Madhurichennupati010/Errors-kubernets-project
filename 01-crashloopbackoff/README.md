# Kubernetes CrashLoopBackOff Practice

## Project Overview

This project demonstrates how I intentionally created a Kubernetes Deployment with an incorrect configuration to generate a **CrashLoopBackOff** error. I then used Kubernetes troubleshooting commands to identify the root cause, fixed the YAML file, and verified that the pod was running successfully.

---

## Objective

- Create a Deployment YAML file.
- Trigger a CrashLoopBackOff error.
- Troubleshoot the issue using Kubernetes commands.
- Fix the YAML configuration.
- Verify the application is running.

---

## Files

```
CrashLoopBackOff/
│
├── README.md
├── deployment.yaml
└── screenshots/
```

---

## Steps Performed

### 1. Created the Deployment YAML

Created a Kubernetes Deployment with an incorrect configuration to simulate a CrashLoopBackOff scenario.

---

### 2. Applied the Deployment

```bash
kubectl apply -f deployment.yaml
```

---

### 3. Verified the Pod Status

```bash
kubectl get pods
```

Observed the pod entering the **CrashLoopBackOff** state.

---

### 4. Investigated the Issue

Checked detailed pod information.

```bash
kubectl describe pod <pod-name>
```

Viewed the application logs.

```bash
kubectl logs <pod-name>
```

---

### 5. Fixed the YAML Configuration

Corrected the error in the `deployment.yaml` file and reapplied the deployment.

```bash
kubectl apply -f deployment.yaml
```

---

### 6. Verified the Fix

Checked the pod status again.

```bash
kubectl get pods
```

The pod transitioned to the **Running** state successfully.

---

## Commands Used

```bash
kubectl apply -f deployment.yaml

kubectl get pods

kubectl describe pod <pod-name>

kubectl logs <pod-name>

kubectl apply -f deployment.yaml

kubectl rollout restart deployment <deployment-name>

kubectl get pods
```

---

## Outcome

- Successfully created a CrashLoopBackOff scenario.
- Identified the root cause using `kubectl describe` and `kubectl logs`.
- Corrected the Deployment YAML.
- Successfully restored the pod to the **Running** state.

---

---

## Key Learnings

- Learned how CrashLoopBackOff occurs.
- Practiced Kubernetes troubleshooting commands.
- Understood how to debug pod failures using logs and events.
- Gained hands-on experience fixing Deployment YAML files.
