# CKAD Learning Repository

> **Goal:** Pass the Certified Kubernetes Application Developer exam by September 2026.
> **Status:** In progress — started July 15, 2026.
> **Cluster:** [kind](kind-config.yaml) (Kubernetes IN Docker), v1.32.2

## Why this exists

I'm a software engineer (Python/Django backend) learning Kubernetes from scratch to:
1. Pass the CKAD certification
2. Build production K8s skills for my next role
3. Document the journey publicly — real commits, real YAML, real mistakes

**Current job:** Full-stack developer at HuQAS (Python/Django/Vue). **Next role target:** Backend/Platform engineer with Kubernetes.

## Certification Details

| Item | Detail |
|---|---|
| Exam | CKAD (Certified Kubernetes Application Developer) |
| Duration | 2 hours, hands-on performance-based |
| Passing score | 66% |
| Cost | $395 (includes one free retake + killer.sh access) |
| Allowed reference | kubernetes.io/docs (one browser tab) |

## Study Plan

| Week | Domain | Status |
|---|---|---|
| 1 | Pods, multi-container patterns, init containers, volumes | ⬜ |
| 2 | Deployments, rolling updates, Jobs, CronJobs | ⬜ |
| 3 | Services (ClusterIP, NodePort), Ingress, NetworkPolicies | ⬜ |
| 4 | ConfigMaps, Secrets, SecurityContexts, RBAC, ServiceAccounts | ⬜ |
| 5 | Probes (liveness/readiness/startup), logging, debugging | ⬜ |
| 6 | Imperative command speed training (50+ exercises) | ⬜ |
| 7 | Helm basics, Kustomize | ⬜ |
| 8–9 | Full mock exams (KillerCoda, Killer.sh) | ⬜ |
| 10 | Exam | ⬜ |

## Repository Structure

```
manifests/
  pods/              # Pod YAML — single, multi-container, init containers
  deployments/       # Deployments, rolling updates, rollbacks
  services/          # ClusterIP, NodePort, LoadBalancer
  configmaps-secrets/# ConfigMaps, Secrets, env vars, volume mounts
  security/          # RBAC, SecurityContexts, ServiceAccounts
  networking/        # NetworkPolicies, Ingress
  jobs-cronjobs/     # Jobs, CronJobs
  probes/            # Liveness, Readiness, Startup probes
  helm-kustomize/    # Helm charts, Kustomize overlays
exercises/           # Exercise logs and solutions
progress/            # Weekly progress summaries
```

## Resources ($0 stack)

| Resource | Link |
|---|---|
| dgkanatsios CKAD Exercises | https://github.com/dgkanatsios/CKAD-exercises |
| KillerCoda CKAD Scenarios | https://killercoda.com/killer-shell-ckad |
| Kubernetes Docs | https://kubernetes.io/docs |
| CKAD Exam Prep (cheatsheets) | https://github.com/ChathurangaVKD/ckad-exam-prep |

## Rules

1. **Never hand-write YAML from scratch.** Generate with `kubectl --dry-run=client -o yaml`, edit in vim, apply.
2. **Namespace discipline.** Set namespace at the start of every exercise: `kubectl config set-context --current --namespace=<ns>`
3. **Use vim for every edit.** No VS Code. The exam terminal only has vim.
4. **Commit daily.** Each study session = at least one commit.
5. **Public repo.** Recruiters should see real Kubernetes work, not just a resume bullet.

---

*Built by [Antony Nyagah](https://github.com/tony-nyagah) — learning in public.*
