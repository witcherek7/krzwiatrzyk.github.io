---
layout: post
title: How to install Kubernetes - step by step
date: 2019-07-14 10:00:00 +0100
description: This post is regarding Kubernetes installation on virtual machines with easiest way possible
img: kubernetes.png
fig-caption: # Add figcaption (optional)
tags: [Kubernetes, K8s]
---

## What is Kubernetes?

Kubernetes is a tool used in cloud computing for deployment automation, increasing scalability and management of containerized applications.
Developed by Google company in 2014 and now developed by Cloud Native Computing Foundation.

## Kubernetes components

+ Kubelet
+ Kubectl
+ Kubeadm


## Kubernetes installation methods

K8s has many different installation methods that differentiate with target environment (Development, Production) and target platform (Bare metal, AWS or cloud in general).

## Method 1 - manually on VM's - Ubuntu 16.04

### Step 1 - /etc/hosts configuration

On each VM open <span style="color:orange">/etc/hosts</i></span> file and fill with addresses and host names of kubernetes nodes.

```
10.0.0.1 master-node
10.0.0.2 slave-node
10.0.0.3 slave-node
```

### Step 2 - set hostname

Set hostname of each VM using command below:

```
hostnamectl set-hostname slave-node
```

### Step 3 - disable swap memory

Kubelet doesn't support swap memory so it has to be disapled in each VM.

Disable swap in current running:
```
swapoff -a
```

Disable it permanently in <span style="color:orange"><i>/etc/fstab</i></span> by commenting the line with UUId after swap:
```
# /etc/fstab: static file system information.
#
# Use 'blkid' to print the universally unique identifier for a
# device; this may be used with UUID= as a more robust way to name devices
# that works even if disks are added and removed. See fstab(5).
#
# <file system> <mount point>   <type>  <options>       <dump>  <pass>
# / was on /dev/sda1 during installation
UUID=9d8a8cd9-3cd7-49cd-ad7b-abf4a41af046 /               ext4    errors=remount-ro 0       1
# swap was on /dev/sda5 during installation
#UUID=17ad87b3-4a65-49a7-b711-c874abebfa7c none            swap    sw              0       0
/dev/fd0        /media/floppy0  auto    rw,user,noauto,exec,utf8 0       0
```

### Step 4 - Install Docker

Docker has to be installed on both master and slaves VM's. 

Below is a full command list to install Docker on Ubuntu 16.0.4 .

> Be wary of docker-ce, docker-ce is provided by docker.com, docker.io is provided by Debian.

[Source](https://stackoverflow.com/questions/45023363/what-is-docker-io-in-relation-to-docker-ce-and-docker-ee)

```
sudo apt-get remove docker docker-engine docker.io

sudo apt-get install apt-transport-https ca-certificates curl software-properties-common -y

sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg | apt-key add -

sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"

sudo apt-get update -y
sudo apt-get install docker-ce -y

sudo systemctl start docker
sudo systemctl enable docker
```

Another way is to install only docker.io.
```
sudo apt-get install docker.io -y

sudo systemctl start docker
sudo systemctl enable docker
```

### Step 5 - Install 