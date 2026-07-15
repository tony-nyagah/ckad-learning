# SecurityContexts, RBAC & ServiceAccounts

## SecurityContexts

### What it is
Security settings applied at the Pod or container level: user/group IDs, capabilities, privilege mode, etc.

### Mental model
SecurityContext is the "run as" parameter — just like `chown` and `chmod` on Linux. You decide which user runs the process, what it's allowed to do, and whether it can escalate.

### Key settings
| Setting | Level | What it does |
|---------|-------|---------------|
| `runAsUser` | Pod/Container | UID to run the process |
| `runAsGroup` | Pod/Container | GID to run the process |
| `fsGroup` | Pod | Group ID for volume ownership |
| `runAsNonRoot` | Pod/Container | Enforce container runs as non-root |
| `privileged` | Container | Run with all capabilities (DANGER) |
| `capabilities.add/drop` | Container | Linux capabilities |
| `readOnlyRootFilesystem` | Container | Root FS is read-only |
| `allowPrivilegeEscalation` | Container | No child process with more privs |

## ServiceAccounts

### What it is
An identity for Pods — determines what they can do within the cluster (API access).

### Key facts
- Every namespace has a `default` ServiceAccount
- Automatically mounted at `/var/run/secrets/kubernetes.io/serviceaccount/` in Pods
- Token is a JWT auto-rotated by the `TokenRequest` API
- Use `automountServiceAccountToken: false` to disable

## RBAC (Role-Based Access Control)

### What it is
Defines who can do what on which resources.

### Mental model
- **Role**: "allowed actions" (what) — permissions on resources in a namespace
- **ClusterRole**: same as Role but cluster-scoped (nodes, PVs, etc.)
- **RoleBinding**: "who gets it" — binds a Role to users/groups/ServiceAccounts (namespace)
- **ClusterRoleBinding**: binds ClusterRoles cluster-wide

### Common commands
| What | Command |
|------|---------|
| Create role | `k create role <name> --verb=get,list,watch --resource=pods` |
| Create clusterrole | `k create clusterrole <name> --verb=get,list --resource=nodes` |
| Create rolebinding | `k create rolebinding <name> --role=<role> --serviceaccount=<ns>:<sa>` |
| Create clusterrolebinding | same, with `k create clusterrolebinding` |
| Check access (auth can-i) | `k auth can-i <verb> <resource> --as=<user>` |

## Gotchas / mistakes I made
-
-

## Exam tips
- `kubectl create role` and `kubectl create rolebinding` are your fastest tools
- `kubectl auth can-i` is critical for verifying RBAC is working
- SecurityContext at Pod level applies to all containers; container-level overrides Pod-level
- ServiceAccount tokens are auto-mounted — disable if not needed
- Drop `ALL` capabilities, then add back only what's needed (least privilege)
