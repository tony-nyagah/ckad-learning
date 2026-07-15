# Pods

## What it is
The smallest deployable unit in Kubernetes. A Pod wraps one or more containers that share network, storage, and lifecycle.

## Mental model
A Pod is a shared apartment — containers inside share the same IP address, port space, and volumes. They can talk to each other via `localhost`.

## Key facts
- Smallest unit you can create and manage
- Containers in a Pod share: network namespace (same IP), IPC namespace, and volumes
- Pods are **ephemeral** — they die and get replaced, new Pod = new IP
- `--restart=Never` creates a bare Pod (not managed by a controller)
- Default restart policy: `Always` (creates a Deployment, not a bare Pod)

## Multi-container patterns
| Pattern | Description | Use case |
|---------|-------------|----------|
| **Sidecar** | Helper container alongside main app | Log shipper, config reloader, proxy |
| **Init container** | Runs to completion before main starts | DB migrations, permission setup, waiting for dependencies |
| **Adapter** | Transforms output for the main container | Normalize logs, transform metrics |
| **Ambassador** | Proxies network traffic | Connect to external databases |

## Common commands
| What | Command |
|------|---------|
| Create bare pod | `k run <name> --image=<img> --restart=Never` |
| Generate pod YAML | `k run <name> --image=<img> --restart=Never --dry-run=client -o yaml > pod.yaml` |
| Get pod YAML | `k get pod <name> -o yaml` |
| Describe (events, status) | `k describe pod <name>` |
| Shell into container | `k exec -it <pod> -- /bin/sh` |
| Shell into specific container | `k exec -it <pod> -c <container> -- /bin/sh` |
| View logs | `k logs <pod>` |
| View logs of specific container | `k logs <pod> -c <container>` |
| Delete pod immediately | `k delete pod <name> --grace-period=0 --force` |
| List pods with labels | `k get pods -l <key>=<value>` |
| Watch pods | `k get pods -w` |

## Gotchas / mistakes I made
-
-

## Exam tips
- Always check `--restart=Never` — omitting it creates a Deployment, not a Pod
- `kubectl explain pod.spec` is your best friend in the exam
- Labels are key for selectors — pick meaningful ones
- Use `--dry-run=client -o yaml` for everything; never hand-write YAML
