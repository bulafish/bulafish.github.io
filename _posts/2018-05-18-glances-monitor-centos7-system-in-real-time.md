---
title: Use Glances to Monitor CentOS7 System in Real Time
author: bulafish
date: 2018-05-18 +0800
categories: [CentOS]
tags: [Ops]
---

[Glances](https://nicolargo.github.io/glances/) is by far the system real time monitoring tool that I like and find handy the most!  It provides lots of system information in one page so I can diagnosis and get the whole picture of the system very quickly.

A glance of glances.  
![glances](/assets/img/2018051802.png)

Glances installation is very simple, you can use yum to install it from repository but the version is older.  
![glances](/assets/img/2018051803.png)

{% include ads3.html %}

Or you can use the command provided by the glances official to install the lastest version.  It will install some required packages to the system first, then finally use `pip` to install glances.
```bash
curl -L https://bit.ly/glances | /bin/bash
```
![glances](/assets/img/2018051801.png)

Finally, enter `glances` at your command line and that's it, enjoy!  You can hit `h` to show more controls.  
![glances](/assets/img/2018051804.png)
