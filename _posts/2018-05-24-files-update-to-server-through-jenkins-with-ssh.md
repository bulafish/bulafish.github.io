---
title: Files Update to Server Through Jenkins with SSH
author: bulafish
date: 2018-05-24 +0800
categories: [DevOps]
tags: [Jenkins]
---

In this post, I am going to configure jenkins server to transfer files to destiny server by ssh.  The goal is link up github, jenkins server and destiny server together.  The steps would be
1. programmers commit codes to github
2. github push notification to trigger jenkins server
3. jenkins server pulls committed codes from github
4. jenkins server deploys codes pulled from github to destiny server

The idea and steps are similar to the image below.  

For steps 1 to 3, please refer: [Jenkins Pull Files from Github Automatically when Files are Commited to Github](https://www.bulafish.com/devops/2018/05/23/auto-pull-files-from-github-to-jenkins-when-commited-to-github/).

Now we begin the construction of last step.  The processes are
1. install necessary plugin
2. setup destiny server.  You can setup as many servers as you need.
3. configure destiny server to jenkins project

Install necessary plugin.  
![jenkins](/assets/images/2018052401.png)  
![jenkins](/assets/images/2018052402.png)  
![jenkins](/assets/images/2018052403.png)  
![jenkins](/assets/images/2018052404.png)  

Re-login jenkins server after plugin installation is done.  Setup destiny server.  
![jenkins](/assets/images/2018052405.png)  
Scroll almost to the bottom of the page to find `Publish over SSH section`.  
![jenkins](/assets/images/2018052406.png)

{% include ads3.html %}

Key in destiny server information.  You can key anything for `Name`, `Hostname` can be ip or FQDN, `Username` is the account used for jeknins to ssh destiny server, `Remote Directory` is the starting location of destiny server.  
![jenkins](/assets/images/2018052418.png)  
Key in the password for the `Username` and give it a test.  Please make sure jenkins server is accessible to destiny server's ssh port.  
![jenkins](/assets/images/2018052408.png)  

Destiny server is done configuring at this moment, next we configure destiny server to jenkins project.  Click into the desired jenkins project.  
![jenkins](/assets/images/2018052409.png)  
![jenkins](/assets/images/2018052410.png)  
![jenkins](/assets/images/2018052411.png)  
Choose the destiny server for project, only one destiny server is shown in the `Name` list cause only one server is built.  \***/*\* for `Source files` meaning all files.  `Remote directory` is the directory on destiny server where you want to codes to be put from jenkins server.  
![jenkins](/assets/images/2018052419.png)  

All settings are done now, take a look at the `/tmp/` of destiny server.  
![jenkins](/assets/images/2018052414.png)

Now we test if everything will work out the way we expected.  You can commit changes to github to trigger jenkins server or simply click deploy on jenkins project.  
![jenkins](/assets/images/2018052420.png)  
![jenkins](/assets/images/2018052421.png)  
![jenkins](/assets/images/2018052422.png)  

Lastly, check on `/tmp/` directory of destiny server to confirm file has been updated successfuly.  
![jenkins](/assets/images/2018052423.png)
