---
layout: post
title: Docker Theory
date: 2019-10-09 8:00:00 +0100
description: Docker theory explained.
img: 
fig-caption: # Add figcaption (optional)
tags: [Docker]
---

# Table of content 

+ ## [What is behind Docker and Containers](#Ch1)
+ ## [Some Facts](#Facts)

# <a name="Ch1">What is behind Docker and Containers</a>

## Base components

Docker and it's containers are able to work thanks to some Linux elements that are able to seperate resources, processes and files.

+ cgroups
+ namespaces
+ /sys filesystem
+ AppArmor and SELinux

## Control groups - cgroups

Controll groups allow to set limits on resources for processes and their children. It can be used to set limits on:
+ memory
+ swap 
+ CPU
+ network I/O
+ storage

Every container is assigned a cgroup that is unique to that container. All processes in the container will be in the same group.

cgroups is also used to accounting of reources usage. In docker this can be shown by *docker stats*.

## /sys

/sys is a filesystem that directly exposes kernel setting and outputs.

It is used by Docker to configure cgroups.

Container settings will be inside: <br>
*/sys/fs/cgroup/cpu/docker/ID*

To update this params *docker container update* can be used.

It is possible to create custom cgroup outside of Docker and attach container to that cgroup using *--cgroup-parent* argument to *docker create*. This mechanism is used by Kubernetes to run multiple containers in one pod with shared quotas.

## Namespaces

They provide to processes some system resources that are generally shared but for them it looks like they have them only for them. Ex. network interface.

Namespaces:
+ mounts - own entire filesystem, separate from host
+ UTS - own hostname and domain name
+ IPC - seperate hosts IPC from containers, like POSIX
+ PID - seperate PID number from host, they can overlap, inside container processes are maped to the host by this namespace
+ network - own network device, port, address
+ user - own user and groups, but root in container is the same on the system

It looks like container is the virtual machine but in fact it work on the shared kernel and is only a process.









# <a name="Facts"> Some Facts </a>

## Docker config file

Daemon.json file is the configuration file for the dockerd server. It can be found in the /etc/docker/ directory. After changing this file it is needed to restart docker.

## Auto-Restarting container

This feature deserves separate chapter.

The `--restart` argument can take four values:
+ no - never restarts
+ always- restart whenever it exits
+ on-failure - whenever exits with a nonzero exit code
+ unless-stopped - restarts always unless it is intentionally stopped with `docker stop`

Example: `docker run --restart=on-failure:3` - docker will try to restart 3 time before giving up


## What Docker Image is?
Docker Image is a basic construct of application. Docker images provide the basis for everything that you will ever deploy and run with Docker. 

## Container related files
In <i>/var/lib/docker/containers</i> are folders with long hashes as names. Those are full container ID's and inside them we can see the files that are used for this container.

## Resources Quota
### CPU Shares
Docker assings the number 1024 to represent full CPU pool. If we want to apply 1/2 of computing power to container we would assign to it 512 shares. 

Constraining the CPU only impacts the application's priority for CPU time.

Example: 2 containers have 512 quota and 1 has 1024. If amount of time for each process in CPU is 100 microseconds then containers with 512 share will get 50 microseconds each time and the one with 1024 will run for 100 microseconds.

### CPU Simplified method

`--cpus=".25"` - command can set a floating-point number beetwen 0.01 and number of CPU cores.

### Memory
Default constraint for memory is a hard limit.




