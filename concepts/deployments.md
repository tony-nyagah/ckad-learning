# Deployments

## What it is
A controller that manages a set of identical Pods, providing declarative updates, scaling, and rollback.

## Mental model
A Deployment is a factory supervisor — you tell it how many workers (replicas) you want and what they should look like (template), and it makes sure that's always true, even if workers quit (Pod dies).

## Key facts
- Manages a ReplicaSet, which manages Pods
- Declarative: you describe desired state, controller makes it happen
- Supports rolling updates (default) and rollbacks
- `kubectl create deployment` auto-generates a Deployment with default settings
- `kubectl run` with `--restart=Always` (default) creates a Deployment

## Update strategies
| Strategy | Behavior |
|----------|----------|
| **RollingUpdate** (default) | Gradually replaces old Pods with new ones, zero downtime |
| **Recreate** | Kills all old Pods first, then creates new ones (downtime) |

Key rolling update params: `maxSurge`, `maxUnavailable`

## Scaling
| What | Command |
|------|---------|
| Scale up/down | `k scale deployment <name> --replicas=<n>` |
| Autoscale | `k autoscale deployment <name> --min=1 --max=10 --cpu-percent=80` |

## Rollout commands
| What | Command |
|------|---------|
| Check rollout status | `k rollout status deployment/<name>` |
| View rollout history | `k rollout history deployment/<name>` |
| Rollback to previous | `k rollout undo deployment/<name>` |
| Rollback to specific revision | `k rollout undo deployment/<name> --to-revision=<n>` |
| Pause rollout | `k rollout pause deployment/<name>` |
| Resume rollout | `k rollout resume deployment/<name>` |
| Restart (new ReplicaSet) | `k rollout restart deployment/<name>` |

## Common commands
| What | Command |
|------|---------|
| Create deployment | `k create deployment <name> --image=<img> --replicas=<n>` |
| Generate deployment YAML | `k create deployment <name> --image=<img> --dry-run=client -o yaml` |
| Set image (triggers update) | `k set image deployment/<name> <container>=<new-image>` |
| Edit live deployment | `k edit deployment/<name>` |
| Get all from deployment | `k get all -l app=<label>` |

## Gotchas / mistakes I made
-
-

## Exam tips
- `kubectl create deployment` + `--dry-run` is faster than writing YAML
- `kubectl set image` triggers a rolling update immediately
- Labels must match between Deployment selector and Pod template
- Use `kubectl explain deployment.spec.strategy` in the exam for rolling update params
