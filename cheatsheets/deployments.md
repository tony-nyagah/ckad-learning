# Deployments — Command Cheatsheet

## Create
```
k create deployment nginx --image=nginx --replicas=3              # deploy with 3 replicas
k create deployment nginx --image=nginx --dry-run=client -o yaml   # generate YAML
k create deployment nginx --image=nginx --port=80                  # with port
```

## Get / List
```
k get deployments
k get deployment nginx -o yaml
k get deployment nginx -o wide
k get rs                                                           # replica sets
k get pods -l app=nginx                                            # pods by deployment label
k get all -l app=nginx                                             # all owned resources
```

## Scale
```
k scale deployment nginx --replicas=5                              # scale up/down
k scale deployment nginx --replicas=5 -n prod                      # in namespace
```

## Update image (triggers rolling update)
```
k set image deployment/nginx nginx=nginx:1.27                      # update image
k edit deployment nginx                                            # vim the live object
```

## Rollout
```
k rollout status deployment/nginx
k rollout history deployment/nginx
k rollout undo deployment/nginx                                    # rollback
k rollout undo deployment/nginx --to-revision=2                    # rollback to specific revision
k rollout pause deployment/nginx                                   # pause updates
k rollout resume deployment/nginx                                  # resume updates
k rollout restart deployment/nginx                                 # restart pods (new ReplicaSet)
```

## Autoscale
```
k autoscale deployment nginx --min=1 --max=10 --cpu-percent=80
```

## Delete
```
k delete deployment nginx
```

## Record (deprecated but might appear)
```
k set image deployment/nginx nginx=nginx:1.27 --record             # save command in history
```
