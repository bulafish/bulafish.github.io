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
















AWS provides a lot of handy services/functions/features to save it's user a lot of time, which includes `Launch VPC Wizard`.

Let me start with the steps to create a AWS VPC with `Launch Wizard` to show you exactly how quickly and easy it can be done.

> If you are using NAT Gateway, allocate an EIP first!

![VPC](/assets/img/Xnip2019-05-27_14-19-11.png)

Confirm to allocate an EIP.

![VPC](/assets/img/Xnip2019-05-27_14-19-33.png)

And that's it! An EIP is allocated to you.

![VPC](/assets/img/Xnip2019-05-27_14-20-00.png)

{% include ads3.html %}

Next we create a VPC by using the Wizard.

![VPC](/assets/img/Xnip2019-05-27_14-21-03.png)

Choose the type of VPC desired.

![VPC](/assets/img/Xnip2019-05-27_14-16-37.png)

At this stage, almost 90% of the configurations are done for you. All you need to do is give your VPC a name and decide whether to use NAT Gateway or instance. Sure you can make changes to any fields according to your needs.

![VPC](/assets/img/Xnip2019-05-27_15-39-20.png)

A few minutes after clicking Create VPC and Ta-da! A VPC is created.

![VPC](/assets/img/Xnip2019-05-27_14-23-30.png)

![VPC](/assets/img/Xnip2019-05-27_14-24-56.png)

An overview of VPC.

![VPC](/assets/img/Xnip2019-05-27_14-26-54.png)

But what I really want to talk about is that, do you really have any idea what the hell you have just done?! If the answer is no, then you should probably think seriously about if this is what you what.

In order for a VPC to work, there are several minimum must-have components and the followings are components that `Launch Wizard` configures for you.

1. Internet Gateway
2. Routing Table
3. Subnets
4. NAT Gateway or instance

Unless you look into the VPC settings and do some cross references after the creation otherwise you won't know what exactly the `Launch Wizard` has done for you behind the curtain.

In conclusion, there are pros and cons of using handy services to accomplish your needs. If you are in a position where you don't need to know how it works behind the scenes, then `Launch Wizard` is definitely the choice. But other than that, it is better that you build a VPC from scratch step by step, to know the dependencies between component as well as the functionality of each of them.