# Volumes

## What it is
Storage that outlives a single container — shared between containers in a Pod.

## Mental model
Containers are stateless by nature. Volumes are external USB drives you plug into the Pod so data survives container restarts.

## Volume types you need to know
| Type | Scope | Use case |
|------|-------|----------|
| **emptyDir** | Pod lifetime | Scratch space, cache, shared between containers |
| **hostPath** | Node filesystem | Access node filesystem (rare in CKAD, used in minikube/kind demos) |
| **configMap** | Read-only config files | Mount config as files |
| **secret** | Read-only secret files | Mount secrets as files |
| **persistentVolumeClaim** | Beyond Pod lifetime | Database storage, persistent data |
| **projected** | Multiple sources | Combine ConfigMap, Secret, downward API, ServiceAccountToken |

## PersistentVolume (PV) & PersistentVolumeClaim (PVC)
| Resource | Role |
|----------|------|
| **PV** | Provisioned storage (admin or dynamic) — a "pool" of storage |
| **PVC** | Pod's request for storage — "I need 5Gi of ReadWriteOnce" |
| **StorageClass** | Defines what kind of storage (ssd, hdd, nfs) and provisioner |

## Access modes
| Mode | Meaning |
|------|---------|
| `ReadWriteOnce (RWO)` | Read/write by single node |
| `ReadOnlyMany (ROX)` | Read-only by many nodes |
| `ReadWriteMany (RWX)` | Read/write by many nodes |
| `ReadWriteOncePod (RWOP)` | Read/write by single Pod |

## Common commands
| What | Command |
|------|---------|
| Create PV | YAML only (apply -f) |
| Create PVC | YAML only (apply -f) |
| Get PV/PVC | `k get pv,pvc` |
| PVC in Pod spec | Add `volumes` + `volumeMounts` to Pod YAML |

## Gotchas / mistakes I made
-
-

## Exam tips
- `emptyDir` data is lost when Pod is deleted (not restarted)
- PV and PVC binding is 1:1 — one PV per PVC
- PVC stays Pending if no matching PV exists (unless StorageClass with dynamic provisioning)
- `volumeMounts.subPath` lets you mount a single file from a ConfigMap/Secret
- Shared volume in multi-container Pods: mount at same path or different paths
