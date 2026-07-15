# Probes & Debugging — Command Cheatsheet

## Probe types quick reference

### httpGet
```yaml
livenessProbe:
  httpGet:
    path: /healthz
    port: 8080
    httpHeaders:
    - name: X-Custom
      value: Awesome
  initialDelaySeconds: 15
  periodSeconds: 10
```

### exec
```yaml
livenessProbe:
  exec:
    command:
    - cat
    - /tmp/healthy
  initialDelaySeconds: 5
  periodSeconds: 5
```

### tcpSocket
```yaml
livenessProbe:
  tcpSocket:
    port: 3306
  initialDelaySeconds: 10
  periodSeconds: 5
```

### startupProbe
```yaml
startupProbe:
  httpGet:
    path: /startup
    port: 8080
  failureThreshold: 30          # up to 30 * 10s = 5 min to start
  periodSeconds: 10
```

## Probe parameters
| Param | Default | Meaning |
|-------|---------|---------|
| initialDelaySeconds | 0 | Wait before first probe |
| periodSeconds | 10 | How often |
| timeoutSeconds | 1 | Timeout |
| failureThreshold | 3 | Failures → action |
| successThreshold | 1 | Successes → success |

## Debugging

### Pod info
```
k describe pod nginx                                              # events, status, containers
k get pod nginx -o yaml                                           # full definition
k get events --sort-by=.metadata.creationTimestamp                # all events
k get events --field-selector involvedObject.name=nginx           # events for a pod
```

### Container debugging
```
k logs nginx -c sidecar --tail=50
k logs nginx --previous                                           # crashed container logs
k exec -it nginx -- /bin/sh                                       # shell in
k exec nginx -- cat /etc/resolv.conf                              # check DNS
k exec nginx -- nslookup service-name                             # check service resolution
k exec nginx -- wget -qO- http://svc-name:port/healthz            # test endpoint
```

### Ephemeral debug container
```
k debug -it nginx --image=busybox --target=nginx                  # attach debug container
k debug node/my-node -it --image=busybox                          # node debugging
```

### Node info
```
k get nodes -o wide
k describe node my-node
k top node
```

### Resource requests/limits
```
k describe pod nginx | grep -A 10 -E "Requests|Limits"
```

### Check if things work
```
k run test --image=busybox --restart=Never --rm -it -- wget -qO- http://service:port
k run test --image=busybox --restart=Never --rm -it -- nslookup service
```
