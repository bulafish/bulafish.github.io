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

![bulafish](/assets/img/Xnip2019-05-27_16-19-07.png)

Setup k8S repo for both master and worker node.

{% gist 257ba4f1c3bafb36433436206df0288d %}

![bulafish](/assets/img/Xnip2019-05-27_16-24-58.png)

```shell
# Set SELinux in permissive mode (effectively disabling it)
setenforce 0
sed -i ‘s/^SELINUX=enforcing$/SELINUX=permissive/’ /etc/selinux/config
```
![bulafish](/assets/img/Xnip2019-05-27_16-25-32.png)

```shell
# Install kubernetes commands
yum install kubelet kubeadm kubectl -y
```

![bulafish](/assets/img/Xnip2019-05-27_16-26-26.png)

![bulafish](/assets/img/Xnip2019-05-27_16-26-37.png)

```shell
# Start kubelet service when rebooting
systemctl enable --now kubelet
```

![bulafish](/assets/img/Xnip2019-05-27_16-27-05.png)

```shell
# Load the br_netfiler module
modprobe br_netfilter
lsmod | grep br_netfilter
```

![bulafish](/assets/img/Xnip2019-05-27_16-27-26.png)

```shell
# To make sure traffic is routed correctly
cat <<EOF > /etc/sysctl.d/k8s.conf
net.bridge.bridge-nf-call-ip6tables = 1
net.bridge.bridge-nf-call-iptables = 1
EOF
```
![bulafish](/assets/img/Xnip2019-05-27_16-27-46.png)

```shell
# Enable the setting
sysctl --system
```

![bulafish](/assets/img/Xnip2019-05-27_16-28-05.png)

```shell
# Add Docker repository
yum-config-manager \
 --add-repo \
 https://download.docker.com/linux/centos/docker-ce.repo
 ```

![bulafish](/assets/img/Xnip2019-05-27_16-28-44.png)

```shell
# Install Docker CE
yum install docker-ce -y
```

![bulafish](/assets/img/Xnip2019-05-27_16-29-09.png)

![bulafish](/assets/img/Xnip2019-05-27_16-29-22.png)

```shell
# Enable, start docker and kubelet service
systemctl enable docker.service
systemctl restart docker
systemctl enable kubelet
systemctl start kubelet
```

![bulafish](/assets/img/Xnip2019-05-27_16-30-23.png)

So far, we have done the basic setups for both master and worker nodes. Next, let’s start to work on the master node.

***



















![bulafish](/assets/img/Xnip2019-05-27_16-19-07.png)

![bulafish](/assets/img/Xnip2019-05-27_16-19-07.png)

![bulafish](/assets/img/Xnip2019-05-27_16-19-07.png)

![bulafish](/assets/img/Xnip2019-05-27_16-19-07.png)

![bulafish](/assets/img/Xnip2019-05-27_16-19-07.png)

![bulafish](/assets/img/Xnip2019-05-27_16-19-07.png)

![bulafish](/assets/img/Xnip2019-05-27_16-19-07.png)

![bulafish](/assets/img/Xnip2019-05-27_16-19-07.png)

![bulafish](/assets/img/Xnip2019-05-27_16-19-07.png)

![bulafish](/assets/img/Xnip2019-05-27_16-19-07.png)

![bulafish](/assets/img/Xnip2019-05-27_16-19-07.png)

![bulafish](/assets/img/Xnip2019-05-27_16-19-07.png)

![bulafish](/assets/img/Xnip2019-05-27_16-19-07.png)

![bulafish](/assets/img/Xnip2019-05-27_16-19-07.png)

