# Pods — Multi-Container (Sidecar Pattern)

A sidecar is a helper container that runs alongside the main app.
They share:
- Network namespace (same IP, can talk via localhost)
- Volume mounts (can share files via emptyDir)

## Example: nginx + log sidecar

The sidecar tails the nginx access log. Both containers share `/var/log/nginx`
via an emptyDir volume.

```bash
kubectl apply -f manifests/pods/sidecar-nginx.yaml
kubectl logs sidecar-nginx -c log-sidecar  # view sidecar logs
```

## Other multi-container patterns to know

| Pattern | What it does | CKAD example |
|---|---|---|
| Sidecar | Helper runs alongside main app | Log shipper, config refresher |
| Ambassador | Proxies traffic to main container | Envoy proxy in front of app |
| Adapter | Transforms output from main container | Normalize log formats |
