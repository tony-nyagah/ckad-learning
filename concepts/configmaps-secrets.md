# ConfigMaps & Secrets

## What it is
ConfigMaps store non-sensitive configuration. Secrets store sensitive data (passwords, tokens, keys). Both decouple config from Pod definitions.

## Mental model
ConfigMaps are sticky notes on the fridge — any Pod can read them. Secrets are in a locked drawer — encoded, but still accessible to anyone with RBAC access to that namespace.

## ConfigMap vs Secret
| | ConfigMap | Secret |
|--|-----------|--------|
| Data | Plain text key-value | Base64-encoded key-value |
| Size limit | 1 MiB | 1 MiB |
| Stored in etcd as | Plain text | Plain text (!) — encode, don't encrypt |
| Use case | App config, env vars, config files | Passwords, API keys, TLS certs |

## Ways to consume
| Method | ConfigMap | Secret |
|--------|-----------|--------|
| Environment variable | ✅ | ✅ |
| Single env var from key | ✅ | ✅ |
| All keys as env vars (`envFrom`) | ✅ | ✅ |
| Volume mount (files) | ✅ | ✅ |
| Volume mount with mode | ✅ | ✅ (permissions) |

## Common commands — ConfigMap
| What | Command |
|------|---------|
| Create from literal | `k create cm <name> --from-literal=<key>=<value>` |
| Create from file | `k create cm <name> --from-file=<path>` |
| Create from env file | `k create cm <name> --from-env-file=<path>` |
| Get configmap | `k get cm <name> -o yaml` |

## Common commands — Secret
| What | Command |
|------|---------|
| Create generic from literal | `k create secret generic <name> --from-literal=<key>=<value>` |
| Create docker-registry | `k create secret docker-registry <name> --docker-server=... --docker-username=...` |
| Create TLS | `k create secret tls <name> --cert=<path> --key=<path>` |
| Get secret (decoded) | `k get secret <name> -o jsonpath='{.data.<key>}' \| base64 -d` |
| Create from file | `k create secret generic <name> --from-file=<path>` |

## Gotchas / mistakes I made
-
-

## Exam tips
- Secrets are base64-encoded, NOT encrypted — anyone with `get` access can decode them
- `envFrom` pulls all keys as env vars at once (vs listing each with `valueFrom`)
- Updating a ConfigMap/Secret does NOT automatically restart Pods consuming it as env vars
- When mounted as a volume, updates are automatically reflected (after a short delay)
- Always use `--from-literal` for quick creation; files for complex config
