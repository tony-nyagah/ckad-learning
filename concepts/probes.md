# Probes (Liveness, Readiness, Startup)

## What it is
Health checks that let Kubernetes know if a container is alive, ready to serve traffic, or still starting up.

## Mental model
- **Liveness**: "Is this container still alive or should I kill it?" (restart if fails)
- **Readiness**: "Is this container ready to receive traffic?" (remove from Service if fails)
- **Startup**: "Is this container still booting up?" (postpone liveness/readiness checks until it passes)

## Probe types
| Type | How it works | Use case |
|------|--------------|----------|
| **exec** | Run a command inside the container. Exit code 0 = success | Custom health script |
| **httpGet** | HTTP GET request. Status 200–399 = success | Web services |
| **tcpSocket** | TCP connection on a port. Connection = success | Non-HTTP services |
| **gRPC** | gRPC health check protocol | gRPC services |

## Key probe parameters
| Param | Default | What it does |
|-------|---------|---------------|
| `initialDelaySeconds` | 0 | Wait before first check |
| `periodSeconds` | 10 | How often to probe |
| `timeoutSeconds` | 1 | Probe timeout |
| `failureThreshold` | 3 | Consecutive failures before action |
| `successThreshold` | 1 | Consecutive successes for success |

## Common patterns
| Pattern | Config |
|---------|--------|
| Fast-app gave liveness too short a delay | Increase `initialDelaySeconds` |
| Graceful shutdown | Long `failureThreshold` in readiness so Service keeps sending traffic |
| Long startup time | Use `startupProbe` with longer `failureThreshold`; liveness/readiness wait |

## Probe YAML examples
```yaml
livenessProbe:
  httpGet:
    path: /healthz
    port: 8080
  initialDelaySeconds: 15
  periodSeconds: 10

readinessProbe:
  exec:
    command:
    - cat
    - /tmp/healthy
  initialDelaySeconds: 5
  periodSeconds: 5

startupProbe:
  httpGet:
    path: /startup
    port: 8080
  failureThreshold: 30
  periodSeconds: 10
```

## Gotchas / mistakes I made
-
-

## Exam tips
- Startup probe runs first; liveness/readiness are disabled until it passes
- Don't make liveness too aggressive — it triggers Pod restarts, which can cascade
- Readiness should be fast and cheap — it gets called constantly while the Pod is serving
- Use `exec` probes sparingly — they spawn a new process each check
