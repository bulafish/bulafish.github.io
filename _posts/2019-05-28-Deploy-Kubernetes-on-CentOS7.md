---
title: Deploy Kubernetes on CentOS7
author: bulafish
date: 2019-05-28 +0800
categories: [K8S]
# tags: []
---

Container has been very popular recently. There are many different tools that can manage/orchestration containers and recently I am looking into [Kubernetes](https://kubernetes.io/). A brief introduction,

> Kubernetes (K8s) is an open-source system for automating deployment, scaling, and management of containerized applications.

Cloud service providers such as AWS, Azure, GCP, have their own kubernetes services such as EKS, AKS and GKE. With those fully managed service, users can get kubernetes service with only a few clicks and not to worry about high availability and elasticity.

But there are still some customers that choose to host/maintain their own kubernetes locally due to some policy/regulation requirements. Therefore this article is going to show you how to build kubernetes step by step on CentOS7.

I have two VMs ready, one for master node, one for worker node.

![VPC](/assets/img/Xnip2019-05-27_16-19-07.png)

Setup k8S repo for both master and worker node.

{% gist 257ba4f1c3bafb36433436206df0288d %}
