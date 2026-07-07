# ErrImagePull / ImagePullBackOff

## Objective

Learn how Kubernetes behaves when an image cannot be downloaded.

---

## Problem

The Pod references an invalid container image.

---

## YAML

```yaml
(Add pod.yaml)
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

Initially

```
ErrImagePull
```

Later

```
ImagePullBackOff
```

---

## Troubleshooting

```bash
kubectl describe pod errimagepull-demo
```

Events

```
Failed to pull image

ErrImagePull

ImagePullBackOff
```

---

## Root Cause

The image name does not exist in the registry.

---

## Resolution

Update the image name to

```
nginx
```

---


