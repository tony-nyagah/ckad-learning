# Services

## What it is
A stable network endpoint (IP + DNS) that routes traffic to a set of Pods selected by labels.

## Mental model
Pods come and go (new IPs). A Service is a bouncer with a guest list — it knows which Pods to route to based on labels, regardless of their IPs. You talk to the bouncer, not individual guests.

## Service types
| Type | Scope | Use case |
|------|-------|----------|
| **ClusterIP** (default) | Internal cluster only | Backend-to-backend communication |
| **NodePort** | Exposes on each node's IP at a static port (30000–32767) | Dev/testing, basic external access |
| **LoadBalancer** | External IP provisioned by cloud provider | Production external access |
| **ExternalName** | CNAME record, no proxies | Redirect to external DNS |

## Key facts
- Selects Pods via labels (`selector` matches `labels`)
- Endpoints are auto-managed based on selector
- Service gets a stable ClusterIP and DNS name: `<service>.<namespace>.svc.cluster.local`
- `targetPort` = container port, `port` = service port
- Headless Service: `clusterIP: None` — no load balancing, returns Pod IPs directly

## Common commands
| What | Command |
|------|---------|
| Expose a pod | `k expose pod <name> --port=<port> --target-port=<target>` |
| Expose a deployment | `k expose deployment <name> --port=<port> --target-port=<target>` |
| Create NodePort service | `k expose deployment <name> --type=NodePort --port=<port>` |
| Get endpoints | `k get endpoints <service>` |
| Describe service | `k describe svc <name>` |

## Gotchas / mistakes I made
-
-

## Exam tips
- `kubectl expose` is the fastest way to create a Service
- Labels on Pods MUST match the Service selector
- Service port names matter when using Ingress or NetworkPolicies
- Port numbers: `port` (Service), `targetPort` (container), `nodePort` (NodePort range)
