# Services & Networking — Command Cheatsheet

## Services

### Create / Expose
```
k expose pod nginx --port=80 --target-port=80                     # expose pod
k expose deployment nginx --port=80 --target-port=8080            # expose deployment
k expose deployment nginx --type=NodePort --port=80               # NodePort
k expose deployment nginx --type=LoadBalancer --port=80           # LoadBalancer
k expose deployment nginx --type=ClusterIP --port=80 --name=svc-name  # named service
k create service clusterip nginx --tcp=80:80                      # imperative clusterip
k create service nodeport nginx --tcp=80:80                       # imperative nodeport
```

### Get / Describe
```
k get svc
k get svc nginx -o yaml
k get svc -o wide
k describe svc nginx
k get endpoints nginx                                              # pod IPs behind service
```

### Delete
```
k delete svc nginx
```

## Ingress

### Create
```
k create ingress my-ingress --rule="host.com/path*=svc:80"        # create rule
k create ingress my-ingress --rule="host.com/*=svc:80,tls=my-tls" # with TLS
```

### Get / Describe
```
k get ingress
k describe ingress my-ingress
```

## NetworkPolicies
```
# No imperative create command — use YAML template
k get networkpolicies
k describe networkpolicy my-policy
```

## Namespace operations
```
k create ns my-ns
k get ns
k delete ns my-ns
k config set-context --current --namespace=my-ns                  # switch namespace
k config view --minify | grep namespace                           # view current ns
```
