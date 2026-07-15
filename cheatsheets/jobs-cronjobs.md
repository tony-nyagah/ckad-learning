# Jobs & CronJobs — Command Cheatsheet

## Jobs

### Create
```
# One-shot
k create job my-job --image=busybox -- /bin/sh -c "echo hello && sleep 5"

# With backoff limit and completion count
k create job my-job --image=busybox --backoff-limit=4 -- /bin/sh -c "echo hello"
```

### Key Job YAML fields
```yaml
spec:
  completions: 5               # how many successful completions
  parallelism: 2               # how many pods run concurrently
  backoffLimit: 4              # max retries (default: 6)
  activeDeadlineSeconds: 100   # time limit for the job
  ttlSecondsAfterFinished: 60  # auto-delete after N seconds
  template:
    spec:
      restartPolicy: OnFailure # or Never
```

### Get / Describe
```
k get jobs
k get job my-job -o yaml
k describe job my-job
```

### Logs
```
k logs job/my-job                                                 # logs of job's pod
k logs -l job-name=my-job                                         # logs by label
```

### Delete
```
k delete job my-job
```

## CronJobs

### Create
```
k create cronjob hello --image=busybox --schedule="*/1 * * * *" -- /bin/sh -c "date; echo hello"
k create cronjob hello --image=busybox --schedule="0 0 * * *" -- /bin/sh -c "echo midnight"
```

### Cron schedule syntax
```
*/5 * * * *     every 5 minutes
0 * * * *       every hour
0 0 * * *       midnight daily
0 0 * * 0       midnight Sunday
*/30 9-17 * * * every 30 min, 9am-5pm
0 0 1 * *       1st of every month
```

### Key CronJob YAML fields
```yaml
spec:
  schedule: "*/1 * * * *"
  startingDeadlineSeconds: 200
  concurrencyPolicy: Forbid      # Allow, Forbid, Replace
  suspend: false
  successfulJobsHistoryLimit: 3
  failedJobsHistoryLimit: 1
```

### Get / Describe
```
k get cronjobs
k get cj                                                           # short name
k describe cronjob hello
```

### Manual trigger
```
k create job hello-manual --from=cronjob/hello                    # ad-hoc run
```

### Suspend / Resume
```
k patch cronjob hello -p '{"spec":{"suspend":true}}'              # suspend
k patch cronjob hello -p '{"spec":{"suspend":false}}'             # resume
```
