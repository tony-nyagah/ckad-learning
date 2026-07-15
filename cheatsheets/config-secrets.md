# ConfigMaps & Secrets — Command Cheatsheet

## ConfigMaps

### Create
```
k create cm app-config --from-literal=ENV=prod --from-literal=DEBUG=false
k create cm app-config --from-file=config.properties
k create cm app-config --from-file=app=app.properties
k create cm app-config --from-env-file=.env
k create cm app-config --from-literal=key1=val1 --dry-run=client -o yaml  # generate YAML
```

### Get / Describe
```
k get cm
k get cm app-config -o yaml
k describe cm app-config
```

### Delete
```
k delete cm app-config
```

## Secrets

### Create
```
k create secret generic db-cred --from-literal=user=admin --from-literal=pass=s3cret
k create secret generic db-cred --from-file=username.txt --from-file=password.txt
k create secret docker-registry regcred --docker-server=my-registry.io --docker-username=user --docker-password=pass --docker-email=email
k create secret tls my-tls --cert=cert.pem --key=key.pem
k create secret generic my-secret --dry-run=client -o yaml          # generate YAML (values in base64)
```

### Get / Describe (careful — secrets shown base64-encoded)
```
k get secrets
k get secret db-cred -o yaml
k describe secret db-cred
k get secret db-cred -o jsonpath='{.data.user}' | base64 -d        # decode value
k get secret db-cred -o jsonpath='{.data}' | while read k v; do echo "$k: $(echo $v | base64 -d)"; done  # decode all
```

## Consuming in Pods — env vars
```yaml
# Env from ConfigMap key
env:
- name: MY_ENV
  valueFrom:
    configMapKeyRef:
      name: app-config
      key: ENV

# Env from Secret key
env:
- name: DB_PASS
  valueFrom:
    secretKeyRef:
      name: db-cred
      key: pass

# All keys as env vars
envFrom:
- configMapRef:
    name: app-config
- secretRef:
    name: db-cred
```

## Consuming in Pods — volumes
```yaml
volumes:
- name: config-vol
  configMap:
    name: app-config
- name: secret-vol
  secret:
    secretName: db-cred

volumeMounts:
- name: config-vol
  mountPath: /etc/config
- name: secret-vol
  mountPath: /etc/secrets
  readOnly: true
```
