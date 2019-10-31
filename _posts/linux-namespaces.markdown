---
layout: post
title: Linux namespaces
date: 2019-10-04 10:00:00 +0100
description: Namespaces, what they are and how to use them.
img: ##
fig-caption: # Add figcaption (optional)
tags: [Linux, namespaces]
---

## What are namespaces?


## How to run command in namespace?

The <i style="color:orange">unshare</i> command allows to un a program with namespaces 'unshared' from its parent.

Root privilages are required to create namespaces.

### Example 

```
$ sudo su                   # become root user
$ hostname                  # check current hostname
dev-ubuntu  
$ unshare -u /bin/sh        # create a shell in new UTS namespace
$ hostname my-new-hostname  # set hostname
$ hostname                  # confirm new hostname
my-new-hostname  
$ exit                      # exit new UTS namespace
$ hostname                  # confirm original hostname unchanged
dev-ubuntu
```

## Namespaces types

+ Mount - isolate filesystem mount points
+ UTS - isolate hostname and domainname
+ IPC - isolate interprocess communication (IPC) resources
+ PID - isolate the PID number space
+ Network - isolate network interfaces
+ User - isolate UID/GID number spaces
+ Cgroup - isolate cgroup root directory




