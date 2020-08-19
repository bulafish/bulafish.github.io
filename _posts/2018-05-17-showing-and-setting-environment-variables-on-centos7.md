---
title: Showing and Setting Environment Variables on CentOS7
author: bulafish
date: 2018-05-17 +0800
categories: [CentOS]
tags: [Ops]
---

For some reason, we set values to Environment variable on our system so that our program can work accordingly.  therefore I learn how to show and set environment variables.

To show all the current environment values,
```bash
env
```
![env](/assets/img/2018051711.png)

To show particular environment value, for example `PATH`,
```bash
echo $PATH
```
![env](/assets/img/2018051712.png)

{% include ads3.html %}

To set a custom environment value, for example `bulafish=hi` and verify,
```bash
export bulafish=hi
echo $bulafish
```
![env](/assets/img/2018051713.png)

The value is only validate for the current ssh session by using the command above.  If you want to set the value permanently, modify `/etc/environment`, after that, you must logout the current ssh connection and re-login again to make the change take effect.  
![env](/assets/img/2018051714.png)

REFERENCES:  
[https://tw.godaddy.com/help/centos-12295](https://tw.godaddy.com/help/centos-12295)  
[Environment Variables](https://www.centoshowtos.org/environment-variables/)
