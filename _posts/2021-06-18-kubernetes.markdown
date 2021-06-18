---
layout: post
title: "Kubernetes for fun and profit"
date: 2021-06-18 10:30:00 -0500
---


Kubernetes for Fun and Profit
=============================

In this series we will be using [Kubespray](https://github.com/kubernetes-sigs/kubespray) to deploy Kubernetes across a few Hyper-V vms. I will say that Hyper-V isnt my first choice in hypervisors, but it is *Not Terrible &trade;* I have found.

Requirements
---
* At least one system designated as the Control Plane (two for redundancy)
* At least two systems designated as nodes
* [Ansible](https://github.com/ansible/ansible)

Overview
---

1. Stand up VMs
2. Install Ansible
3. `git clone` the Kubespray repo
4. Setup your inventory
5. ???
6. Profit!
