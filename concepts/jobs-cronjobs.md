# Jobs & CronJobs

## Jobs

### What it is
Creates one or more Pods and ensures a specified number of them terminate successfully.

### Mental model
A Job is a task runner — you say "run this thing until it completes N times," and it keeps trying until success or the backoff limit is reached.

### Key facts
- Pods get `restartPolicy: OnFailure` or `Never`
- `completions`: how many successful Pod completions needed
- `parallelism`: how many Pods run concurrently
- `backoffLimit`: how many retries before Job is marked failed
- `activeDeadlineSeconds`: time limit for the Job
- `ttlSecondsAfterFinished`: auto-cleanup completed Jobs

### Job patterns
| Pattern | Config |
|---------|--------|
| One-shot task | `completions: 1`, `parallelism: 1` (default) |
| Parallel fixed completions | `completions: N`, `parallelism: M` |
| Parallel work queue | `parallelism: N`, no `completions` (one Pod per work item) |

## CronJobs

### What it is
A Job on a schedule — uses cron syntax to create Jobs at specified times.

### Key facts
- Creates a new Job each time the schedule fires
- `schedule`: cron expression (min hour day month day-of-week)
- `startingDeadlineSeconds`: deadline for starting a missed Job
- `concurrencyPolicy`: `Allow` (default), `Forbid`, or `Replace`
- `suspend`: pause the CronJob without deleting it
- `successfulJobsHistoryLimit` / `failedJobsHistoryLimit`: retain N completed/failed Jobs

## Common commands
| What | Command |
|------|---------|
| Create Job | `k create job <name> --image=<img> -- <command>` |
| Create CronJob | `k create cronjob <name> --image=<img> --schedule="*/1 * * * *" -- <command>` |
| Get Jobs | `k get jobs` |
| Get CronJobs | `k get cronjobs` |
| View Job logs | `k logs job/<name>` |
| Trigger CronJob manually | `k create job --from=cronjob/<name> <temp-name>` |

## Gotchas / mistakes I made
-
-

## Exam tips
- `restartPolicy: OnFailure` allows retries within a Pod; `Never` creates new Pods
- Cron schedule: 5 fields. Practice them. `*/5 * * * *` = every 5 min, `0 0 * * *` = midnight
- `kubectl create job --from=cronjob/<name>` creates an ad-hoc run
- `concurrencyPolicy: Replace` is useful for long-running jobs to avoid overlap
