# CrashLoopBackOff

## Objective

Understand why a container repeatedly crashes and how to troubleshoot it.

---

## Problem

The container exits immediately with Exit Code 1.

---

## YAML

```yaml
(Add your pod.yaml here)
```

---

## Deploy

```bash
kubectl apply -f pod.yaml
```

---

## Verify

```bash
kubectl get pods
```

Status

```
CrashLoopBackOff
```

---

## Troubleshooting

```bash
kubectl logs crashloop-demo

kubectl logs crashloop-demo --previous

kubectl describe pod crashloop-demo
```

---

## Root Cause

The container command executes:

```
echo Starting...
exit 1
```

Since the main process exits with Exit Code 1, Kubernetes continuously restarts the container.

---

## Resolution

Remove the custom command so the nginx container starts normally.

---

## Screenshot

Add screenshots from

```
screenshots/crashloopbackoff/
```
