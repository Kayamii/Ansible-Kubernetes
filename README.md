# Kubernetes Ansible Setup
## Getting Started

First, clone the repository to your local machine:

```bash
git clone https://github.com/Kayamii/Ansible-Kubernetes.git
cd Ansible-Kubernetes
```
## Table of Contents
- [Introduction](#introduction)
- [Prerequisites](#prerequisites)
- [Installation Steps](#installation-steps)
  - [Library Fixes](#library-fixes)
  - [Controller Setup](#controller-setup)
  - [Master and Worker Setup](#master-and-worker-setup)
- [Configuration Files](#configuration-files)
  - [Config-one-by-one](#Config-one-by-one)
  - [Config-all-in-one](#Config-all-in-one)
  - [k8s.yml](#k8syml)
  - [Inventory File](#inventory-file)
- [Usage](#usage)
- [Conclusion](#conclusion)

## Repository Structure

```plaintext
repo/
├── inventory
├── k8s.yml                   # Script YAML for Kubernetes deployment
├── k8s_worker_node_connection.j2  # Jinja2 template for worker node connection
└── Remote_Files/
  └── worker_conn_string    # Connection string output from the join command
```

## Introduction
This documentation provides a comprehensive guide to setting up a Kubernetes cluster using Ansible. It includes steps for fixing libraries, configuring the controller, and setting up master and worker nodes.

## Prerequisites
- A Linux environment (Ubuntu preferred)
- Ansible installed on the controller node
- SSH access between the controller and nodes
- Basic knowledge of YAML and Ansible
- **Three machines** with the following specifications(minimum):
  - **2 GB RAM**
  - **2 CPU cores**
  - **10 GB STORAGE**
  - **Ubuntu Live Server** or any Ubuntu image installed
- Ensure all machines can communicate with each other:
  - One controller node
  - One master node
  - One or more worker nodes

## Installation Steps

### Library Fixes
(If you face one of these errors, follow these steps; otherwise, skip to the next part)

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
You will find everything above.

This file contains the Ansible playbook that sets up Kubernetes prerequisites and installs Kubernetes components. Below is an overview of the tasks included.

### Config-one-by-one
  For a modular setup, execute the scripts individually in the following order:

  ```bash
  ansible-playbook -i inventory setup_prerequisites.yml
  ansible-playbook -i inventory setup_controller.yml
  ansible-playbook -i inventory join_workers.yml
  ```

### Config-all-in-one
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
code from https://github.com/ChristianLempa/boilerplates/blob/main/ansible/kubernetes/README.md
