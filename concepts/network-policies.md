# NetworkPolicies

## What it is
A firewall rule for Pods — controls what traffic is allowed IN (ingress) and OUT (egress) based on labels, namespaces, and IP blocks.

## Mental model
A NetworkPolicy is a security guard at each Pod's door — by default, all doors are open (all traffic allowed). Adding a policy closes the door and only lets through traffic matching the rules you define.

## Key facts
- Requires a **Network Plugin** (CNI) that supports NetworkPolicies (e.g., Calico, Cilium, Weave)
- By default: pods accept ALL traffic
- Once a NetworkPolicy selects a pod: only traffic matching rules is allowed (default deny for selected pods)
- Policies are additive (multiple policies = union of rules)

## Policy structure
| Field | Purpose |
|-------|---------|
| `podSelector` | Which Pods this policy applies to |
| `policyTypes` | `Ingress`, `Egress`, or both |
| `ingress.from` | Allowed sources (podSelector, namespaceSelector, ipBlock) |
| `egress.to` | Allowed destinations |

## Selector types
| Selector | Matches |
|----------|---------|
| `podSelector` | Labels on Pods (same namespace) |
| `namespaceSelector` | Labels on Namespaces |
| `ipBlock` | CIDR ranges |
| Combination: `podSelector` + `namespaceSelector` | Pods in specific namespaces |

## Common commands
| What | Command |
|------|---------|
| Create NetworkPolicy | Use `kubectl apply -f` (no imperative shortcut) |
| Get NetworkPolicies | `k get networkpolicies` |
| Describe NetworkPolicy | `k describe networkpolicy <name>` |

## Gotchas / mistakes I made
-
-

## Exam tips
- No `kubectl create` imperative command exists for NetworkPolicies — have a YAML template ready
- CNI plugin must support NetworkPolicies (kind default CNI — kindnet — does NOT)
- When you apply the first policy to a pod, all other traffic is denied
- Multiple NetworkPolicies are combined (union), not overridden
- `ipBlock` only works in the `from`/`to` section, not as a `podSelector`
