---
title: MySQL80 Installation From MySQL Repository On CentOS7
author: bulafish
date: 2018-05-06 +0800
categories: [Database]
tags: [MySQL]
---

MySQL has recently announced the release of MySQL80 and seems like a lot of improvement has been made.  So let's start with installing it from official's repository.

You can get appropriate repository file from [MySQL :: Download MySQL Yum Repository](https://dev.mysql.com/downloads/repo/yum/).  To install it,
```bash
yum install https://dev.mysql.com/get/mysql80-community-release-el7-1.noarch.rpm
```
![mysql80 installation](/assets/img/2018050610.png)

Two .repo files will be placed after installation.  
![mysql80 installation](/assets/img/2018050611.png)

With these two files, we can start MySQL80 installation.
```bash
yum install mysql-community-server
```
![mysql80 installation](/assets/img/2018050612.png)

Start mysql service and check status after installation.
```bash
systemctl start mysqld
systemctl status mysqld
```
![mysql80 installation](/assets/img/2018050613.png)

{% include ads3.html %}

Next, we need to obtain the temporary password first initializing mysql service.
```bash
cat /var/log/mysqld.log | grep "temporary password"
```
![mysql80 installation](/assets/img/2018050614.png)

Now we start mysql initialization.
```bash
mysql_secure_installation
```
![mysql80 installation](/assets/img/2018050615.png)  
![mysql80 installation](/assets/img/2018050616.png)

Now we are all set, login to mysql with user `root` and the password you just set.
```bash
mysql -u root -p
```
![mysql80 installation](/assets/img/2018050617.png)

You should be able to login if everything was setup correctly.  And to wrap everything up with couple commands to interact with mysql server and everything works fine and as expected!