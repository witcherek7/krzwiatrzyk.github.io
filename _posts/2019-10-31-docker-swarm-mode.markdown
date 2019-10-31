---
layout: post
title: Docker Swarm mode
date: 2019-10-17 8:00:00 +0100
description: Docker Swarm mode.
img: 
fig-caption: # Add figcaption (optional)
tags: [Docker]
---

## Docker Swarm

Docker Swarm was and orchestrator published by Docker Inc. but wasn't such succesful as Kubernetes. It required installation of seperate package which was a huge driveback regarding it could be installed with Docker Engine. Docker Swarm mode hovewer is installed with Docker Engine and is replacing the original Docker Swarm.

## Docker Swarm mode

Docker allows to controle several docker workers with usage of swarm managers (there can be multiple ones! redundancy!).

To start swarm manager:
```
docker swarm init --advertise-addr 192.168.0.1
```

After that docker will give us command to put into worker nodes to join to this manager.

To get again token needed to add workers:
```
docker swarm join-token --quiet worker
```

To check host info, manager or not:
```
docker -H 192.168.0.1 info
```

Disclaimer: 192.168.0.1 - active manager IP

To list all hosts in the cluster:
```
docker -H 192.168.0.1 node ls
```

Create default network for services:
```
docker -H 192.168.0.1 network create --driver=overlay default-net
```

Launching a service (container):
```
docker -H 192.168.0.1 service create --detach=true --name myservice --replicas 2 --publish published=80,target=8080 --network default-net myimage:latest
```

Check running services and where they are (on which hosts):
```
docker -H 192.168.0.1 service ps myservice
docker -H 192.168.0.1 service ls
```

Inspect service:
```
docker -H 192.168.0.1 service inspect --pretty myservice
```

Update order - how to scale service when new version needs to be deployed:
+ N-1 - destroy one old and replace with new one
+ N+1 - create new one before destroying old one

The same principle can be done with rollbacks.

To change it:
```
docker -H 192.168.0.1 service update --update-order=start-first/stop-first --rollback-order=start-first/stop-first myservice
```

To scale up/down service:
```
docker -H 192.168.0.1 service scale --detach=False myservice=5
```

Update example:
```
docker -H 192.168.0.1 service update --update-delay 10s \
    --update-failure-action rollback --update-monitor 5s \
    --update-order start-first --update-parallelism 1 \
    --detach=false \
    --image myservice:latest2 myservice
```


To update one container at the time:
```
--update-parallelism 1
```


How to rollback:
```
docker -H 192.168.0.1 service rollback myservice
```

To move all task from node that will be decomisionned or in need of downtime:
```
docker -H 192.168.0.1 node update --availability drain ip-192.168.0.2
```

To add this node again:
```
docker -H 192.168.0.1 node update --availability active ip-192.168.0.2
```

To remove the service:
```
docker -H 192.168.0.1 service rm myservice
```

