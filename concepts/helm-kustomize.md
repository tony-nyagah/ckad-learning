# Helm & Kustomize

## Helm

### What it is
A package manager for Kubernetes — templated YAML with values, packaged as "charts."

### Mental model
Helm is like `apt` or `brew` for Kubernetes. A chart is a package, values are configuration, and a release is an installed instance.

### Key concepts
- **Chart**: a directory with templates, default values, and metadata
- **Release**: an instance of a chart running in a cluster
- **Repository**: a collection of charts (like Docker Hub for charts)
- **Values**: YAML that overrides chart defaults

### Common commands
| What | Command |
|------|---------|
| Search for charts | `helm search hub <keyword>` |
| Add repo | `helm repo add <name> <url>` |
| Install chart | `helm install <release> <chart> --set key=value` |
| Upgrade release | `helm upgrade <release> <chart>` |
| List releases | `helm list` |
| Get release values | `helm get values <release>` |
| Rollback | `helm rollback <release> <revision>` |
| Uninstall | `helm uninstall <release>` |
| Create chart | `helm create <name>` |
| Template (render) | `helm template <name> <chart> -f values.yaml` |

## Kustomize

### What it is
A native Kubernetes tool (built into `kubectl`) for customizing YAML without templates — uses overlays, patches, and generators.

### Mental model
Kustomize is a layering system. You have a **base** (common config) and **overlays** (environment-specific changes). Think of it as "patches on top of defaults," no templating needed.

### Key concepts
- **Base**: common resources shared across environments
- **Overlay**: environment-specific customizations (dev, staging, prod)
- **kustomization.yaml**: the config file that ties it all together

### Common commands
| What | Command |
|------|---------|
| Build (render) | `kubectl kustomize <dir>` |
| Apply directly | `kubectl apply -k <dir>` |
| Set image | `kustomize edit set image <name>=<new-image>` |
| Set namespace | `kustomize edit set namespace <ns>` |
| Add resource | `kustomize edit add resource <file>` |

### Common transformers/patches
| Transform | What it does |
|-----------|---------------|
| `namePrefix` / `nameSuffix` | Add prefix/suffix to all resource names |
| `commonLabels` | Add labels to all resources |
| `commonAnnotations` | Add annotations to all resources |
| `images` | Override image names/tags |
| `patchesStrategicMerge` | Patch specific resources |
| `namespace` | Set namespace for all resources |

## Helm vs Kustomize
| Aspect | Helm | Kustomize |
|--------|------|-----------|
| Approach | Templating (Go templates) | Patching / overlays |
| Learning curve | Steeper (template syntax) | Gentler (pure YAML) |
| Built into kubectl | No (separate CLI) | Yes (`kubectl apply -k`) |
| Packaging | Charts (versioned, repos) | No packaging concept |
| Best for | Distributing apps | Environment customization |

## Gotchas / mistakes I made
-
-

## Exam tips
- CKAD tests basic usage only — install, upgrade, list, rollback for Helm; `-k` flag for Kustomize
- `helm install` vs `helm upgrade --install` — the latter is idempotent
- `kubectl apply -k` is all you need for Kustomize; `kubectl kustomize` for preview
- Kustomize overlays go in separate directories with their own `kustomization.yaml`
