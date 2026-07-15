# Ingress

## What it is
A collection of routing rules that control how external HTTP/HTTPS traffic reaches Services inside the cluster.

## Mental model
An Ingress is a smart router at the cluster gate — it reads the URL path or hostname and directs traffic to the right Service, like a reverse proxy. Without it, you'd need a LoadBalancer per Service.

## Key facts
- Requires an **Ingress Controller** (e.g., nginx, traefik) — Ingress resources are just rules
- Routes by: host, path, or both
- Supports TLS termination
- Ingress → Service → Pods

## Anatomy of a rule
```yaml
spec:
  rules:
  - host: app.example.com
    http:
      paths:
      - path: /api
        pathType: Prefix
        backend:
          service:
            name: api-service
            port:
              number: 80
```

## Common commands
| What | Command |
|------|---------|
| Create ingress | `k create ingress <name> --rule="host/path=svc:port"` |
| Get ingress | `k get ingress` |
| Describe ingress | `k describe ingress <name>` |

## Gotchas / mistakes I made
-
-

## Exam tips
- `kubectl create ingress` with `--rule` is the fastest path
- Check Ingress Controller is installed — Ingress won't work without one
- PathType: `Prefix` matches all subpaths, `Exact` matches only that path
- Default backend: handles requests that don't match any rule
