---
title: Deploy Kubernetes on CentOS7
author: bulafish
date: 2019-05-28 +0800
categories: [K8S]
# tags: []
---

Container has been very popular recently. There are many different tools that can manage/orchestration containers and recently I am looking into [Kubernetes](https://kubernetes.io/){:target="_blank"}. A brief introduction,

> [Kubernetes (K8s)](https://kubernetes.io/docs/concepts/overview/what-is-kubernetes/) is an open-source system for automating deployment, scaling, and management of containerized applications.

Cloud service providers such as AWS, Azure, GCP, have their own kubernetes services such as [EKS](http://images/flower.png%20Kubernetes%20(K8s)%20is%20an%20open-source%20system%20for%20automating%20deployment,%20scaling,%20and%20management%20of%20containerized%20applications.), [AKS](https://azure.microsoft.com/en-us/services/kubernetes-service/) and [GKE](https://azure.microsoft.com/en-us/services/kubernetes-service/). With those fully managed service, users can get kubernetes service with only a few clicks and not to worry about high availability and elasticity.

But there are still some customers that choose to host/maintain their own kubernetes locally due to some policy/regulation requirements. Therefore this article is going to show you how to build kubernetes step by step on CentOS7.

I have two VMs ready, one for master node, one for worker node.

![bulafish](/assets/img/Xnip2019-05-27_16-19-07.png)

Setup k8S repo for `both` master and worker node.

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

{% include ads3.html %}

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

So far, we have done the basic setups for `both` master and worker nodes. Next, let’s start to work on the `master node`.

***

```shell
# Initial master node

kubeadm init
```

![bulafish](/assets/img/Xnip2019-05-27_16-30-49.png)

After initialization, copy down the `kubeadm join` command. It is needed to join worker nodes into kubernetes cluster.

![bulafish](/assets/img/Xnip2019-05-27_16-32-51.png)

```shell
# If you are running as root, enter this command in order for kubeadm commands to work

export KUBECONFIG=/etc/kubernetes/admin.conf
```

![bulafish](/assets/img/Xnip2019-05-27_16-33-09.png)

```shell
# Deploy a pod network to the cluster

kubectl apply -f "https://cloud.weave.works/k8s/net?k8s-version=$(kubectl version | base64 | tr -d '\n')"
```

> *For more detail and options about pot network, please refer [pod-network](https://kubernetes.io/docs/setup/independent/create-cluster-kubeadm/#pod-network)*

![bulafish](/assets/img/Xnip2019-05-27_16-33-36.png)

***

Now let’s return to worker node. If you have the following error msg when issuing `kubeadm join`, you can do the following steps to solve it.

> *\[WARNING IsDockerSystemdCheck\]: detected “cgroupfs” as the Docker cgroup driver. The recommended driver is “systemd”. Please follow the guide at <https://kubernetes.io/docs/setup/cri/>*

> `*If you have the above error msg when running **kubeadm join** command, then do the following setups.*`

```shell
# Install necessary packages

yum install yum-utils device-mapper-persistent-data lvm2
```

![bulafish](/assets/img/Xnip2019-05-27_21-55-30.png)

![bulafish](/assets/img/Xnip2019-05-27_21-55-38.png)

```shell
# Create /etc/docker directory & setup daemon
# Restart docker service

mkdir /etc/docker
cat > /etc/docker/daemon.json <<EOF
{
  "exec-opts": ["native.cgroupdriver=systemd"],
  "log-driver": "json-file",
  "log-opts": {
    "max-size": "100m"
  },
  "storage-driver": "overlay2",
  "storage-opts": [
    "overlay2.override_kernel_check=true"
  ]
}
EOF

mkdir -p /etc/systemd/system/docker.service.d
systemctl daemon-reload
systemctl restart docker
```

![bulafish](/assets/img/Xnip2019-05-27_21-57-25.png)

```shell
# Finally, add worker node to kubernetes cluster by using the command generated from master node after kubeadm init command

kubeadm join 172.31.39.53:6443 --token 2nkiam.xxxxx --discovery-token-ca-cert-hash sha256:266c7d0a89f26976fa8b5952f6xxxxx
```

> *It is very import to make sure that worker node is allowed to connect to worker node 6443 port!*

![bulafish](/assets/img/Xnip2019-05-27_22-13-40.png)

***

The worker node has been successfully added to kubernetes cluster. Now back to `master node` to confirm that the worker node is up and running.

```shell
# Get the list and status of nodes
kubectl get nodes
```

![bulafish](/assets/img/Xnip2019-05-27_22-19-33.png)

From the image, you can see that the worker node is `Ready`, meaning up and running. Till now, we have successfully done setting up the kubernetes cluster with a node added to it.

The last part is to deploy some containers to verify if the cluster works correctly. From master node,

```shell
# Create a container running sample image at port 8080
kubectl run node-hello --image=gcr.io/google-samples/node-hello:1.0  --port=8080

# Expose pod to outside world, external ip is the local ip of node server
kubectl expose deployment.apps/node-hello --type="NodePort" --port 8080 --external-ip=172.31.40.107
```

![bulafish](/assets/img/Xnip2019-05-27_23-33-32.png)

Now a sample container is deployed, use a browser to check the result!

![bulafish](/assets/img/Xnip2019-05-27_23-36-18.png)