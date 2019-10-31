---
layout: post
title: Ansible breakdown
date: 2019-10-14 10:00:00 +0100
description: How to install ansible, configure it and build playbooks
img: #
fig-caption: # Add figcaption (optional)
tags: [Ansible]
---

## Preparing Ansible configuration file

Configuration file is in path: <i>/etc/ansible/ansible.cfg</i>

Every option we activate we have to put after [defaults] tag.

To disable checking if SSH key is saved on the host, if it was at least once connected through SSH or key was maunaly added.
```
[defaults]
host_key_checking = False
```

To disable some warning about deprecations that can be realy annoying.
```
[defaults]
deprecation_warnings = False
```

To make debug and faults more readable.
```
[defaults]
stdout_callback=debug
stderr_callback=debug
```

## Ansible inventory file

Ansible executes tasks on hosts from inventory file. This file can be specified globaly or locally per project/playbook.

Global inventory file is in <i>/etc/ansible/hosts</i>

Example hosts file:
```
[k8s]
k8master ansible_connection=ssh ansible_ssh_host=192.168.0.50 ansible_port=22 ansible_ssh_user=krzwiatrzyk ansible_password=cisco123 ansible_become_password=cisco123
k8node1 ansible_ssh_host=192.168.0.51 ansible_ssh_port=22 ansible_password=cisco123 ansible_user=krzwiatrzyk ansible_become_password=cisco123
k8node2 ansible_ssh_host=192.168.0.52 ansible_ssh_port=22 ansible_password=cisco123 ansible_user=krzwiatrzyk ansible_become_password=cisco123
```

To run ansible command with local inventory file type:
```
ansible -i hosts.txt -m command all
```


## Ansible execution commands examples

```
ansible-playbook kubernetes-install.yml 
ansible -m ping <host/group>
absible-playbook playbook.yml --start-at-task="name-of-the task"
ansible-playbook playbook.yml --step
```

## Solution for SSH key changing

```
echo "" > ~/.ssh./known_hosts
```

## Repisotiry list file

```
/etc/apt/sources.list
```


