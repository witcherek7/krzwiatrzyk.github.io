---
layout: post
title: How to install Kubernetes - Ansible
date: 2019-10-08 10:00:00 +0100
description: My personall prepared playbook for deploying kubernetes with ansible.
img: kubernetes.png
fig-caption: # Add figcaption (optional)
tags: [Kubernetes, K8s]
---

## Ansible execution command

```
ansible-playbook kubernetes-install.yml 
ansible -m ping <host/group>
absible-playbook playbook.yml --start-at-task="name-of-the task"
ansible-playbook playbook.yml --step
```

### Kubernetes commands

```
kubeadm reset
kubectl get pods
```

## Solution for SSH key changing

```
echo "" > ~/.ssh./known_hosts
```

## Repisotiry list file

```
/etc/apt/sources.list
```


## Intro

This tutorial is based on my local setup. 

k8master - 192.168.0.50 <br>
k8node1 - 192.168.0.51 <br>
k8node2 - 192.168.0.52 

## Preparation

All Ansible nodes and master should have SSH installed and enabled.

```
sudo apt-get install -y openssh-server
sudo systemctl enable ssh
sudo systemctl start ssh

sudo route add default gw 192.168.0.1
sudo ifconfig ens33 192.168.0.50 netmask 255.255.255.0 
sudo ifconfig ens33 192.168.0.51 netmask 255.255.255.0
sudo ifconfig ens33 192.168.0.51 netmask 255.255.255.0
```

Also it is good to add static IP addresses to all the nodes to not change.


## Ansible installation

1. Adding project's PPA (personal package archive) to the system.

```
sudo apt-add-repository ppa:ansible/ansible
```

2. Refresh system's package index and install ansible.

```
sudo apt-get update
sudo apt-get install ansible
```

## Ansible configuration 

1. Add managed hosts on:

```
sudo nano /etc/ansible/hosts
```

In my setup it looks like this:
```
[k8s]
k8master ansible_connection=local  ansible_ssh_host=192.168.0.50
k8node1 ansible_ssh_host=192.168.0.51 ansible_ssh_port=22 ansible_password=cisco123 ansible_user=krzwiatrzyk
k8node2 ansible_ssh_host=192.168.0.52 ansible_ssh_port=22 ansible_password=cisco123 ansible_user=krzwiatrzyk
```
Keeping password in plain text isn't a good practice but we will handle with it later.

# !!! - FIRST EXECUTE MASTER PLAYBOOK 
Also put your username as 
```
user: krzwiatrzyk
```
in <i>variables</i> file alongside playbooks.

## Ansible nodes playbook

```
---
- hosts: k8nodes
  vars_files:
  - variables
  #become: yes

  tasks:

  - name: Change Hostname
    shell: hostnamectl set-hostname {{inventory_hostname}}
    become: yes

  - name: Disabling Swap
    shell: swapoff -a
    become: yes

  - name: Disabling swap partition  
    replace:
      path: /etc/fstab
      regexp: '^(.+?\sswap\s+sw\s+.*)$'
      replace: '# \1'
    become: yes

  - name: Update /etc/hosts file with entries
    lineinfile: 
      dest: /etc/hosts
      line: "{{hostvars[item].ansible_ssh_host}} {{hostvars[item].inventory_hostname}}"
      state: present
    with_items: "{{groups.k8s}}"
    become: yes

  - name: Install Docker
    apt: 
      name: docker.io 
    become: yes 

  - name: Enable Docker service
    systemd:
      name: docker
      enabled: yes

  - name: Install packages that allow apt to be used over HTTPS
    apt:
      name: "{{ packages }}"
      state: present
      update_cache: yes
    become: yes
    register: out
    vars:
      packages:
      - apt-transport-https
      - ca-certificates
      - curl
      - gnupg-agent
      - software-properties-common

  - debug: 
      var: out.stdout_lines

  - name: Add an apt signing key for Kubernetes
    become: yes
    apt_key:
      url: https://packages.cloud.google.com/apt/doc/apt-key.gpg
      state: present

  - name: Adding apt repository for Kubernetes
    become: yes
    apt_repository:
      repo: deb http://packages.cloud.google.com/apt/ kubernetes-xenial main
      state: present
      filename: kubernetes.list

  - name: Install Kubernetes binaries
    become: yes
    apt: 
      name: "{{ packages }}"
      state: present
      update_cache: yes
    vars:
      packages:
        - kubelet 
        - kubeadm 
        - kubectl

  - name: Copy join command script to node 
    copy:
      src: join_command.sh
      dest: /home/{{user}}/join_command.sh
      owner: krzwiatrzyk
      group: krzwiatrzyk
      mode: '0777'
    become: yes

  - name: Execute join command
    command: "sh /home/{{user}}/join_command.sh"
    become: yes
```

## Ansible master playbook
```
---
- hosts: k8master
  vars_files:
  - variables
  #become: yes

  tasks:

  - name: Change Hostname
    shell: hostnamectl set-hostname k8master
    become: yes

  - name: Disabling Swap
    shell: swapoff -a
    become: yes

  - name: Disabling swap partition  
    replace:
      path: /etc/fstab
      regexp: '^(.+?\sswap\s+sw\s+.*)$'
      replace: '# \1'
    become: yes

  - name: Update /etc/hosts file with entries
    lineinfile: 
      dest: /etc/hosts
      line: "{{hostvars[item].ansible_ssh_host}} {{hostvars[item].inventory_hostname}}"
      state: present
    with_items: "{{groups.k8s}}"
    become: yes

  - name: Install Docker
    apt: 
      name: docker.io 
    become: yes 

  - name: Enable Docker service
    systemd:
      name: docker
      enabled: yes

  - name: Install packages that allow apt to be used over HTTPS
    apt:
      name: "{{ packages }}"
      state: present
      update_cache: yes
    become: yes
    register: out
    vars:
      packages:
      - apt-transport-https
      - ca-certificates
      - curl
      - gnupg-agent
      - software-properties-common

  - debug: 
      var: out.stdout_lines

  # - name: Add user to docker group
  #   user:
  #     name: "{{ user }}"
  #     group: docker

  - name: Add an apt signing key for Kubernetes
    become: yes
    apt_key:
      url: https://packages.cloud.google.com/apt/doc/apt-key.gpg
      state: present

  - name: Adding apt repository for Kubernetes
    become: yes
    apt_repository:
      repo: deb http://packages.cloud.google.com/apt/ kubernetes-xenial main
      state: present
      filename: kubernetes.list

  - name: Install Kubernetes binaries
    become: yes
    apt: 
      name: "{{ packages }}"
      state: present
      update_cache: yes
    vars:
      packages:
        - kubelet 
        - kubeadm 
        - kubectl

  # - name: Create kubelet file
  #   file:
  #     path: "/etc/default/kubelet"
  #     state: touch

  # - name: Configure node ip
  #   lineinfile:
  #     path: /etc/default/kubelet
  #     line: KUBELET_EXTRA_ARGS=--node-ip=192.168.0.50

  # - name: Restart kubelet
  #   service:
  #     name: kubelet
  #     daemon_reload: yes
  #     state: restarted

  - name: Initialize the Kubernetes cluster using kubeadm
    command: kubeadm init --pod-network-cidr=10.0.0.0/16 
    become: yes   

  - name: Setup kubeconfig for krzwiatrzyk user
    command: "{{ item }}"
    become: yes
    with_items:
      # - mkdir -p /home/{{user}}/.kube
      # - cp -i /etc/kubernetes/admin.conf /home/{{user}}/.kube/config
      # - chown {{user}}:{{user}} /home/{{user}}/.kube/config
      - mkdir -p /home/{{user}}/.kube
      - cp /etc/kubernetes/admin.conf /home/{{user}}/.kube/config
      - chown {{user}}:{{user}} /home/{{user}}/.kube/config

    # - name: Export Kubernetes config for repository
    #   command: export KUBECONFIG=/etc/kubernetes/admin.conf
    #   become: true

  - name: Download calico network file
    get_url:
      url: https://docs.projectcalico.org/v3.8/manifests/calico.yaml
      dest: /home/{{user}}/calico.yaml

  - name: Replace calico network pool
    replace: 
      path: /home/{{user}}/calico.yaml
      regexp: '192.168.0.0'
      replace: '10.0.0.0'

  - name: Install calico pod network
    become: no
    command: kubectl apply -f /home/{{user}}/calico.yaml

  - name: Generate join command
    command: kubeadm token create --print-join-command
    register: join_command

  - name: Copy join command to local file
    local_action: copy content="{{ join_command.stdout_lines[0] }}" dest="./join-command.sh"
```
