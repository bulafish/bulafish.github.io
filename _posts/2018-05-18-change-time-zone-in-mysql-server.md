---
title: Change Time Zone in MySQL Server
author: bulafish
date: 2018-05-18 +0800
categories: [Database]
tags: [MySQL]
---

Due to some incidence so I learned  that MySQL server has its own time zone cofiguration.  Following is the way to modify MySQL server's time according to your need.

First to check the current time MySQL server.
```mysql
select curtime();
```
![mysql](/assets/images/mysql-time-1.jpg)

To config the time, modify `/etc/my.cnf` and add the variable `default-time-zone`.  
![mysql](/assets/images/mysql-time-2.jpg)

{% include ads3.html %}

Save the change, restart MySQL service.  
![mysql](/assets/images/mysql-time-3.jpg)

Re-login to MySQL server and check if server time is changed.  
![mysql](/assets/images/mysql-time-4.jpg)
