---
title: Install Jenkins on CentOS7
author: bulafish
date: 2018-05-18 +0800
categories: [DevOps]
tags: [Installation, Jenkins]
---

[CI](https://en.wikipedia.org/wiki/Continuous_integration) / [CD](https://en.wikipedia.org/wiki/Continuous_delivery) has been very popular lately and [Jenkins](https://jenkins.io/) is just one of the [CI tools](https://en.wikipedia.org/wiki/Comparison_of_continuous_integration_software).  There are two ways to install Jenkins, one is to [download](https://jenkins.io/download/) the .war file directory, another way is to install through [yum](https://wiki.jenkins.io/display/JENKINS/Installing+Jenkins+on+Red+Hat+distributions).  In this post, I will use the later way.

Install jenkins's repository file follow by install jenkins and java.
```bash
wget -O /etc/yum.repos.d/jenkins.repo http://pkg.jenkins-ci.org/redhat-stable/jenkins.repo
rpm --import https://jenkins-ci.org/redhat/jenkins-ci.org.key
yum install jenkins java
```
![jenkins](/assets/img/2018051814.png)  
![jenkins](/assets/img/2018051815.png)  
![jenkins](/assets/img/2018051816.png)

Start Jenkins service.
```bash
systemctl start jenkins
systemctl status jenkins
```
![jenkins](/assets/img/2018051817.png)

Jenkins use 8080 port by default, we can double check by using `ss`.
```bash
ss -tpln
```
![jenkins](/assets/img/2018051818.png)

{% include ads3.html %}

You need to make sure port 8080 is accessible.  For simplicity, I will stop firewalld for now.  For more information about firewalld, please refer: [CentOS7 firewall-cmd Cheat Sheet](https://www.bulafish.com/centos/2018/04/27/centos7-firewalld-cheat-sheet/)
```bash
systemctl stop firewalld
```
![jenkins](/assets/img/2018051828.png)

Now use browser to open Jenkins.  Type in `yourserverip:8080` or `yoururl:8080`.  
![jenkins](/assets/img/2018051820.png)

As suggested, admin password can be found at `/var/lib/jenkins/secrets/initialAdminPassword`
```bash
cat /var/lib/jenkins/secrets/initialAdminPassword
```
![jenkins](/assets/img/2018051821.png)


After passwd is enter, now we start to install plugins.  
![jenkins](/assets/img/2018051822.png)

Plugins installing.  
![jenkins](/assets/img/2018051823.png)

After plugins installation, we can decide to create a new user or continue as admin.  For simplicity, I will continue as admin.  
![jenkins](/assets/img/2018051824.png)

Last step is to save the configuration.  
![jenkins](/assets/img/2018051825.png)  
![jenkins](/assets/img/2018051826.png)

Once clicked on start using jenkins, you are in Jenkins!  
![jenkins](/assets/img/2018051827.png)
