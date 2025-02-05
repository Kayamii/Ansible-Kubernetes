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
This documentation provides a comprehensive guide to setting up a Kubernetes cluster using Ansible. It includes steps for fixing libraries, configuring the controller, and setting up master and worker nodes.

## Prerequisites
- A Linux environment (Ubuntu preferred)
- Ansible installed on the controller node
- SSH access between the controller and nodes
- Basic knowledge of YAML and Ansible

## Installation Steps

### Library Fixes
Before beginning the setup, ensure that the necessary libraries are properly configured:

```bash
sudo rm /var/lib/dpkg/lock-frontend
sudo rm /var/lib/dpkg/lock
sudo dpkg --configure -a
dpkg --print-foreign-architectures
sudo dpkg --remove-architecture i386
sudo apt-get autoremove
sudo apt-get update
