# Kubernetes Ansible Setup

## Table of Contents
- [Introduction](#introduction)
- [Prerequisites](#prerequisites)
- [Installation Steps](#installation-steps)
  - [Library Fixes](#library-fixes)
  - [Controller Setup](#controller-setup)
  - [Master and Worker Setup](#master-and-worker-setup)
- [Configuration Files](#configuration-files)
  - [k8s.yml](#k8syml)
  - [Inventory File](#inventory-file)
- [Usage](#usage)
- [Conclusion](#conclusion)

## Introduction
This documentation provides a comprehensive guide to setting up a Kubernetes cluster using Ansible. It includes steps for fixing libraries, configuring the controller, and setting up master and worker nodes.

## Prerequisites
- A Linux environment (Ubuntu preferred)
- Ansible installed on the controller node
- SSH access between the controller and nodes
- Basic knowledge of YAML and Ansible

## Installation Steps

### Library Fixes
(if u face one of these errors follow this , else skip to the next part)
Before beginning the setup, ensure that the necessary libraries are properly configured:

```bash
sudo rm /var/lib/dpkg/lock-frontend
sudo rm /var/lib/dpkg/lock
sudo dpkg --configure -a
dpkg --print-foreign-architectures
sudo dpkg --remove-architecture i386
sudo apt-get autoremove
sudo apt-get update
```

### Controller Setup
Update package lists:

```bash
sudo apt-add-repository ppa:ansible/ansible
sudo apt update
sudo apt install ansible
ansible --version
ssh-keygen
```

Copy SSH keys to master and worker nodes:

```bash
ssh-copy-id master@ip_master
ssh-copy-id worker@ip_worker
```

Enable password authentication:

```bash
sudo nano /etc/ssh/sshd_config
# Add or change the line:
PasswordAuthentication yes
```

### Master and Worker Setup
On the master and worker nodes, install OpenSSH server:

```bash
sudo apt update
sudo apt install openssh-server
sudo systemctl stop ufw
sudo systemctl disable ufw
```

## Configuration Files

### k8s.yml
This file contains the Ansible playbook that sets up Kubernetes prerequisites and installs Kubernetes components. Below is an overview of the tasks included.

```yaml
- name: Setup Prerequisites To Install Kubernetes
  hosts: workers,masters
  become: true
  vars:
    kube_prereq_packages: [curl, ca-certificates, apt-transport-https]
    kube_packages: [kubeadm, kubectl, kubelet]

  tasks:
    - name: Test Reachability
      ansible.builtin.ping:

    # Additional tasks...
```

### Inventory File
The inventory file defines the nodes in the Kubernetes cluster.

```ini
[kubernetes]
192.168.100.97 ansible_ssh_user=master
192.168.100.98 ansible_ssh_user=worker1

[masters]
192.168.100.97 ansible_ssh_user=master

[workers]
192.168.100.98 ansible_ssh_user=worker1
```

## Usage
Test connectivity to all nodes:

```bash
ansible all -i inventory -m ping 
```

If the connectivity test is successful, run the Kubernetes setup script:

```bash
ansible-playbook -i inventory k8s.yml --ask-become-pass
```

## Conclusion
This documentation provides a structured approach to setting up a Kubernetes cluster using Ansible. Follow the steps carefully to ensure a smooth installation process.
