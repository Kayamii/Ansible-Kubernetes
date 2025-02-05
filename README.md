# Ansible-Kubernetes
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
- [License](#license)

## Introduction
This repository contains a comprehensive guide on setting up a Kubernetes cluster using Ansible. The documentation covers the prerequisite requirements, installation steps, and configuration instructions for both master and worker nodes.

## Prerequisites
- Linux environment (Ubuntu preferred)
- Ansible installed on the controller node
- SSH access between the controller and nodes
- Basic knowledge of YAML and Ansible

## Installation Steps

### Library Fixes
Before proceeding with the setup, make sure the necessary libraries and packages are configured.

```bash
sudo rm /var/lib/dpkg/lock-frontend
sudo rm /var/lib/dpkg/lock
sudo dpkg --configure -a
dpkg --print-foreign-architectures
sudo dpkg --remove-architecture i386
sudo apt-get autoremove
sudo apt-get update
