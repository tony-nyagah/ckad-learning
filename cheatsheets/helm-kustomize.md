# Helm & Kustomize — Command Cheatsheet

## Helm

### Repo commands
```
helm repo add bitnami https://charts.bitnami.com/bitnami
helm repo update
helm repo list
helm search hub nginx                                               # search all repos
helm search repo nginx                                              # search added repos
```

### Install / Upgrade
```
helm install my-app bitnami/nginx                                   # install
helm install my-app bitnami/nginx --set service.type=NodePort       # with values
helm install my-app bitnami/nginx -f values.yaml                    # from values file
helm install my-app ./my-chart                                      # from local chart
helm upgrade my-app bitnami/nginx                                   # upgrade
helm upgrade --install my-app bitnami/nginx                         # idempotent (install if not exists)
helm install my-app bitnami/nginx --dry-run                         # dry run
```

### Management
```
helm list                                                           # list releases
helm list -A                                                        # all namespaces
helm get values my-app                                              # current values
helm get manifest my-app                                            # rendered YAML
helm status my-app
helm history my-app
helm rollback my-app 1                                              # rollback to revision 1
helm uninstall my-app
```

### Chart development
```
helm create my-chart                                                # scaffold a chart
helm template my-app ./my-chart -f values.yaml                      # render without installing
helm lint ./my-chart                                                # validate chart
helm package ./my-chart                                             # create .tgz
```

## Kustomize

### Common commands
```
kubectl kustomize <dir>                                             # render (show output)
kubectl apply -k <dir>                                              # apply directly
kubectl delete -k <dir>                                             # delete resources
```

### kustomization.yaml examples
```yaml
# Base
apiVersion: kustomize.config.k8s.io/v1beta1
kind: Kustomization
namespace: my-ns

resources:
- deployment.yaml
- service.yaml

commonLabels:
  app: my-app
  env: prod

commonAnnotations:
  owner: team-platform

images:
- name: nginx
  newTag: 1.27

patches:
- path: replicas-patch.yaml
```

```yaml
# Overlay
apiVersion: kustomize.config.k8s.io/v1beta1
kind: Kustomization

bases:
- ../../base

namespace: production

patchesStrategicMerge:
- increase-replicas.yaml
```

### Edit commands
```
cd overlay/ && kustomize edit set image nginx=nginx:1.27
cd overlay/ && kustomize edit set namespace production
cd overlay/ && kustomize edit add resource ../../base/deployment.yaml
cd overlay/ && kustomize edit add label env:prod
```

### Directory structure pattern
```
├── base/
│   ├── kustomization.yaml
│   ├── deployment.yaml
│   └── service.yaml
└── overlays/
    ├── dev/
    │   └── kustomization.yaml
    └── prod/
        ├── kustomization.yaml
        └── increase-replicas.yaml
```
