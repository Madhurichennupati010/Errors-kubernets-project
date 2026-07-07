# ErrImagePull / ImagePullBackOff

## Overview

This lab demonstrates how a Pod enters the **ErrImagePull** state when Kubernetes fails to pull the specified container image from the container registry. After repeated failed attempts, Kubernetes changes the Pod status to **ImagePullBackOff** and increases the delay between pull attempts.

This is a common issue in production environments caused by incorrect image names, invalid image tags, private repositories without authentication, or registry connectivity issues.

---

## Objective

- Understand the difference between **ErrImagePull** and **ImagePullBackOff**.
- Learn how Kubernetes pulls container images.
- Practice troubleshooting image pull failures.
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
  name: errimagepull-demo
spec:
  containers:
  - name: demo-container
    image: nginx/madhu
```

---

## Deploy the Pod

```bash
kubectl apply -f errimagepull.yaml
```

Expected Output

```text
pod/errimagepull-demo created
```

---

## Verify Pod Status

```bash
kubectl get pods
```

Initially, you may see:

```text
NAME                  READY   STATUS          RESTARTS   AGE
errimagepull-demo     0/1     ErrImagePull    0          10s
```

After a few seconds:

```text
NAME                  READY   STATUS               RESTARTS   AGE
errimagepull-demo     0/1     ImagePullBackOff     0          30s
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
kubectl describe pod errimagepull-demo
```

Example Output

```text
Events:

Pulling image "nginx/madhu"

Failed to pull image "nginx/madhu"

Error: ErrImagePull

Back-off pulling image "nginx/madhu"

Error: ImagePullBackOff
```

---

### 3. Verify the Image Name

```bash
kubectl get pod errimagepull-demo -o yaml
```

Output

```yaml
image: nginx/madhu
```

The specified image does not exist in the Docker registry.

---

## Root Cause

The Pod references an invalid container image:

```text
nginx/madhu
```

During Pod creation, Kubernetes attempts to pull the image from the container registry. Since the repository does not exist (or access is denied), the image pull fails.

The Pod first enters the **ErrImagePull** state.

After repeated failed pull attempts, Kubernetes applies an exponential backoff strategy and changes the Pod status to **ImagePullBackOff**.

---

## Resolution

Update the image name to a valid image.

Example:

```yaml
image: nginx
```

or

```yaml
image: nginx:latest
```

Delete the existing Pod:

```bash
kubectl delete pod errimagepull-demo
```

Deploy the updated Pod:

```bash
kubectl apply -f errimagepull.yaml
```

Verify:

```bash
kubectl get pods
```

Expected Output

```text
NAME                  READY   STATUS    RESTARTS   AGE
errimagepull-demo     1/1     Running   0          15s
```

---

## Kubernetes Commands Used

```bash
kubectl apply -f errimagepull.yaml

kubectl get pods

kubectl describe pod errimagepull-demo

kubectl get pod errimagepull-demo -o yaml

kubectl delete pod errimagepull-demo

kubectl apply -f errimagepull.yaml
```

---

## Key Learnings

- Learned how Kubernetes pulls container images.
- Understood the difference between **ErrImagePull** and **ImagePullBackOff**.
- Used `kubectl describe pod` to identify image pull failures.
- Identified invalid image names as the root cause.
- Resolved the issue by updating the image name.

---

## Difference Between ErrImagePull and ImagePullBackOff

| ErrImagePull | ImagePullBackOff |
|--------------|------------------|
| First image pull attempt fails. | Kubernetes repeatedly retries pulling the image. |
| Indicates the initial failure to download the image. | Indicates Kubernetes is waiting before the next retry (backoff). |
| Usually appears first. | Appears after multiple failed pull attempts. |

---

## Interview Questions

### Q1. What is ErrImagePull?

**Answer:**

`ErrImagePull` indicates that Kubernetes attempted to pull the container image from the registry but the operation failed. Common causes include incorrect image names, invalid tags, private repositories without authentication, or network issues.

---

### Q2. What is ImagePullBackOff?

**Answer:**

`ImagePullBackOff` occurs after repeated image pull failures. Kubernetes waits for progressively longer intervals before retrying the image pull to avoid overwhelming the registry. This delay mechanism is called **exponential backoff**.

---

### Q3. How do you troubleshoot an ImagePullBackOff error?

**Answer:**

1. Check the Pod status.

```bash
kubectl get pods
```

2. Describe the Pod.

```bash
kubectl describe pod errimagepull-demo
```

3. Review the **Events** section for image pull errors.

4. Verify the image name and tag.

5. If using a private registry, ensure the correct image pull secret is configured.

6. Update the image name or credentials and redeploy the Pod.

---

## Project Outcome

This lab demonstrates practical Kubernetes troubleshooting by simulating an image pull failure, analyzing Kubernetes events, understanding the transition from **ErrImagePull** to **ImagePullBackOff**, and resolving the issue by correcting the container image.
