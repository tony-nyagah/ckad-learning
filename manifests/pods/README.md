# Pods — Single Container

## Basic nginx pod (generated imperatively)

```bash
kubectl run nginx --image=nginx --restart=Never --dry-run=client -o yaml > pod.yaml
```

## Pod with custom command

```bash
kubectl run busybox --image=busybox --restart=Never --dry-run=client -o yaml -- /bin/sh -c "sleep 3600"
```

## Expose port on pod

```bash
kubectl run nginx-exposed --image=nginx --restart=Never --port=80
```

## Key lessons learned

- `kubectl run` without `--restart=Never` creates a **Deployment**, not a Pod
- `--dry-run=client -o yaml` generates YAML without creating the resource — your cheat code
- `kubectl set image pod/<name> <container>=<image>:<tag>` kills and recreates the container
- Pods get auto-injected with Kubernetes service env vars (KUBERNETES_SERVICE_HOST, etc.)
