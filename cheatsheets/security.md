# Security — Command Cheatsheet

## RBAC

### Roles
```
k create role pod-reader --verb=get,list,watch --resource=pods -n my-ns
k create role pod-reader --verb=get,list --resource=pods,services -n my-ns
k create clusterrole node-reader --verb=get,list --resource=nodes
```

### RoleBindings
```
k create rolebinding pod-reader-bind --role=pod-reader --user=jane -n my-ns
k create rolebinding pod-reader-bind --role=pod-reader --serviceaccount=my-ns:my-sa -n my-ns
k create rolebinding pod-reader-bind --role=pod-reader --group=devs -n my-ns
k create clusterrolebinding node-reader-bind --clusterrole=node-reader --user=jane
```

### Check permissions
```
k auth can-i get pods --as=jane -n my-ns                           # check as user
k auth can-i create deployments --as=system:serviceaccount:my-ns:my-sa
k auth can-i list nodes --as=jane
k auth can-i '*' '*' --as=jane                                     # all verbs, all resources
```

### Get
```
k get roles -n my-ns
k get rolebindings -n my-ns
k get clusterroles
k get clusterrolebindings
k describe role pod-reader -n my-ns
```

## ServiceAccounts
```
k create sa my-sa -n my-ns
k get sa
k describe sa my-sa
k get sa my-sa -o yaml
```

## SecurityContext — Pod level
```yaml
spec:
  securityContext:
    runAsUser: 1000
    runAsGroup: 3000
    fsGroup: 2000
    runAsNonRoot: true
```

## SecurityContext — Container level
```yaml
containers:
- name: app
  securityContext:
    runAsUser: 1000
    allowPrivilegeEscalation: false
    readOnlyRootFilesystem: true
    capabilities:
      drop:
      - ALL
      add:
      - NET_BIND_SERVICE
```

## Verify
```
k exec my-pod -- whoami                                              # check user
k exec my-pod -- id                                                  # check uid, gid
k exec my-pod -- touch /test                                         # test read-only filesystem
```
