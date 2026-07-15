# Pods & Containers — Command Cheatsheet

## Create
```
k run nginx --image=nginx --restart=Never                        # bare pod
k run nginx --image=nginx --restart=Never --dry-run=client -o yaml  # generate YAML
k run nginx --image=nginx --restart=Never --dry-run=client -o yaml > pod.yaml  # save to file
k run nginx --image=nginx:1.26 --restart=Never --port=80        # with port
k run nginx --image=nginx --restart=Never -l tier=front          # with labels
k run busybox --image=busybox --restart=Never -- sleep 3600     # with command
k run nginx --image=nginx --restart=Never --env=ENV=prod        # with env var
```

## Get / List
```
k get pods                                                         # list
k get pods -o wide                                                 # with node IP, pod IP
k get pods -l tier=front                                           # by label
k get pods --show-labels                                           # show labels column
k get pods -L tier                                                 # show tier label as column
k get pods --sort-by=.metadata.creationTimestamp                   # sorted
k get pods --field-selector=status.phase=Running                   # only running
k get pods -A                                                      # all namespaces
k get pod nginx -o yaml                                            # full definition
k get pod nginx -o jsonpath='{.spec.containers[*].image}'         # specific field
k get pod nginx -o wide                                            # node + IP
```

## Describe
```
k describe pod nginx
```

## Logs
```
k logs nginx                                                       # default container
k logs nginx -c sidecar                                            # specific container
k logs nginx --tail=20                                             # last 20 lines
k logs nginx -f                                                    # follow
k logs nginx --since=1h                                            # since time
k logs -l tier=front                                               # all with label
k logs nginx --previous                                            # previous crashed container
```

## Exec
```
k exec -it nginx -- /bin/sh                                        # shell into pod
k exec -it nginx -c sidecar -- /bin/sh                             # specific container
k exec nginx -- ls /app                                            # run command, no TTY
k exec nginx -- env                                                # print env vars
```

## Delete
```
k delete pod nginx
k delete pod nginx --grace-period=0 --force                        # force kill
k delete pods -l tier=front                                        # by label
k delete pod nginx --wait=false                                    # don't wait
```

## Labels / Annotations
```
k label pod nginx env=prod                                         # add label
k label pod nginx env=prod --overwrite                             # overwrite
k label pod nginx env-                                             # remove label
k annotate pod nginx description="web server"                      # add annotation
```

## Image operations
```
k set image pod/nginx nginx=nginx:1.27                             # change image (not common for bare pods)
```

## Port forward
```
k port-forward pod/nginx 8080:80                                   # local:pod
```

## Copy files
```
k cp pod/nginx:/etc/nginx/nginx.conf ./nginx.conf                  # from pod to local
k cp ./config.txt pod/nginx:/etc/config.txt                        # from local to pod
```

## Top / Metrics
```
k top pod                                                          # resource usage (requires metrics-server)
k top pod --sort-by=cpu
k top pod --sort-by=memory
```
