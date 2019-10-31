---
layout: post
title: Docker commands
date: 2019-09-13 8:00:00 +0100
description: What docker Image is and what instructions are used to build it.
img: 
fig-caption: # Add figcaption (optional)
tags: [Docker]
---

# Table of content

+ ## General commands
+ ## Monitoring commands
+ ## Image commands
+ ## Container commands
+ ## Quota commands
+ ## Volume commands
+ ## Network commands
+ ## Command params




# Commands

## dockerd commands
+ `--userns-remap` - forces docker to run withis a user an group context that is unprivileged on the host system

## General commands

+ `docker info` - shows information about docker deamon
+ `docker export`  - export container files into the specific path 
<br>
Example: <i>`docker export dfs1233dsf -o application.tar`</i>


+ `docker version` - client, server, API version
+ `docker info` - OS, Kernel version, number of containers and images
+ `docker prune` - removes all stopped containers, not used networks, dangling images, build cache
+ `docker prune -a` - removes also all unused images
+ `docker rm $(docker ps -a -a)` - example combination of commands


## Monitoring commands
+ `docker ps -a -f label=dev=Chris` - search containers for matching label
+ `docker inspect CONTAINER` - displays informations about container, including labels
+ `docker history IMAGE` - shows built layers and their size
+ `docker events` - display docker deamon logs
+ `docker logs CONTAINER` - show container output saved by docker


## Image commnds

+ `docker create IMAGE sleep 120` - creates and image that will shut down after 120 seconds
+ `docker history IMAGE --no-trunc` - shows layers based on the container is built with whole commands
+ `docker diff ID` - checking which files were added (A) or changed (C) in container compared to image

## Container commnds

+ `docker run IMAGE -l dev=Chris` - creates container with label dev

+ `docker run IMAGE --hostname="linux"` - creates container with custom hostname instead of hash

+ `docker run IMAGE --dns=8.8.8.8 --dns-search=ex1.com --dns-search=ex2.com` - creates container and overrides default resolv.conf file that is copied from the host OS

+ `docker run IMAGE --mac-address="xx:xx:xx:xx:xx:xx"` - sets MAC address for container

+ `docker run IMAGE -v local/folder:/container/folder:ro,z` - mounts volume from host to container with read-only permissions. If any of the mount points don't exist they will be created.
<br>
Uppercase Z indicates that bind mount content is private and unshared. Lowercase z indicates that bind mount content is shared among multiple containers. 
<br>
To check: <i>`mount | grep data`</i>

+ `docker run IMAGE --read-only=true` - restrict container to save any file inside of it. It will be possible to save to one location if we will mount volume with permissions RW.
+ `docker run IMAGE --tmpfs /tmp:rw,noexec,nodev,nosuid,size=256M` - binds tmp folder that will be deleted after container stops
+ `docker run --oom-kill-disable --om-score-adj` - disables Linux OOM killer which kills a process when requested RAM is higher than available quota
+ `docker pause/unpause` - pauses container without killing process
+ `docker stop` - sends SIGTERM signal and waiting for the container to exit gracefully


## Quota commands

+ `docker container update` - allows to change resource quotas for running container
<br>
Example: `docker update --cpus"1.5" CONTAINER`

+ `docker run IMAGE --cpu-shares 512 --cpuset=0,1` - cpu params
+ `docker run IMAGE --memory 512m` - memory params, constrain to 512 of RAM and 512 MB of addtional swap space
+ `docker run IMAGE --memory=512m --memory-swap=768m` - setting swap separatily, setting `--memory-swap` to -1 will turn off swap withing a container


## Volumes commands:

+ `docker volume` - list volumes stored in root directory, those are not volume bindings done in container creation
+ `docker volume create my-docker-volume` - creating volume in docker, useful when data needs to be shared beetwen containers
+ `docker volume inspect` - volume inspect
+ `docker run --mount source=my-docker-volume,target=/app` - mounting craeted volume

## Network commands:

+ `docker network ls` - lists all used networks
+ `docker network inspect` - show details about networks and also containers attached to them

## Scaling commands:

+ `docker run --restart=on-failure:3` - docker will try to restart 3 time before giving up

## Command params

+ `--no-trunc` - will not shorten any info, commands will be shown full, container id will be full



