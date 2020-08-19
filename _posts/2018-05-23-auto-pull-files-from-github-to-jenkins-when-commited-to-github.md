---
title: Jenkins Pull Files from Github Automatically when Files are Commited to Github
author: bulafish
date: 2018-05-23 +0800
categories: [DevOps]
tags: [Jenkins]
---

In this post, I am going to write about how to interact jenkins and github so that whenever files are commited to github, it will trigger jenkins to pull the files from github.  To install jenkins server on CentOS7, please refer: [Install Jenkins on CentOS7](https://www.bulafish.com/devops/2018/05/18/install-jenkins-on-centos7/)

In this post, we need
1. A jenkins server, with required packages installed.
2. github repository, public or private.  For simplicity, I will use public repository.

Let's start with installing necessary packages for jenkins server.
```bash
yum install git
```
![jenkins](/assets/images/2018052323.png)

Get ready a github repository.  For simplicity, I just have a readme file for testing later.  
![jenkins](/assets/images/2018052324.png)

Install plugins for jenkins server since it needs to interact with github.   
![jenkins](/assets/images/2018052325.png)  
![jenkins](/assets/images/2018052326.png)  
Click available, then filter for github.  
![jenkins](/assets/images/2018052327.png)  
Then scroll down to find `GitHub Integration`, check it and click.  
![jenkins](/assets/images/2018052328.png)  
Jenkins will now start installing the plugin, check the box at bottom to restart jenkins server.  
![jenkins](/assets/images/2018052329.png)  
![jenkins](/assets/images/2018052330.png)

Jenkins server will restart after installation, you will be prompted to login jenkins server again.  Now we make a jenkins project, before that, we need to get the url of the github repository which to interact with jenkins server first.  
![jenkins](/assets/images/2018052331.png)

Now start to create jenkins project.  
![jenkins](/assets/images/2018052332.png)  
Click ok at bottom once is done.  
![jenkins](/assets/images/2018052333.png)

{% include ads3.html %}

Check github project and post the copied url, please note the `.git` at the end of url is removed!  
![jenkins](/assets/images/2018052334.png)  
Here, do not remove the `.git` at the end of the url.  
![jenkins](/assets/images/2018052335.png)  
The check means that to notify jenkins server whenever the repository previously set was modified.  
![jenkins](/assets/images/2018052336.png)  
Jenkins server are done configuring, next we start to configure github settings.  
![jenkins](/assets/images/2018052337.png)  
Click on Add webhook.  
![jenkins](/assets/images/2018052338.png)  
Enter the url of jenkins server, must follow by `github-webhook/`.  
![jenkins](/assets/images/2018052339.png)  
Please make sure your jenkins server's 8080 port is accessible by github webhook servers.  Github [ip ranges](https://api.github.com/meta).  To modify firewall in CentOS7, please refer: [CentOS7 firewall-cmd Cheat Sheet](https://www.bulafish.com/centos/2018/04/27/centos7-firewalld-cheat-sheet/).  If everything is setup correctly, you will see a green tick as picture shown below.  
![jenkins](/assets/images/2018052340.png)

Now everything is setup, we can start to test the process.  The idea is whenever codes in github is committed, github will automatically push a notification to trigger jenkins server to pull files from github, so we manually modify the readme file and commit it to trigger the notification.  
![jenkins](/assets/images/2018052341.png)  
![jenkins](/assets/images/2018052342.png)

Switch to jenkins homepage right after committed on github repository, you will see a job is on queue.  
![jenkins](/assets/images/2018052343.png)

Then the queue start to build.  Click on it will lead to buld page.  
![jenkins](/assets/images/2018052344.png)  
![jenkins](/assets/images/2018052347.png)

Click on Output then all the build process will be shown.  
![jenkins](/assets/images/2018052345.png)

After build, click on Workspace to confirm jenkins has pull the updated file from github repository.  
![jenkins](/assets/images/2018052348.png)  
![jenkins](/assets/images/2018052349.png)  
![jenkins](/assets/images/2018052350.png)

Next:  
[Files Update to Server Through Jenkins with SSH ](https://www.bulafish.com/devops/2018/05/24/files-update-to-server-through-jenkins-with-ssh/)
