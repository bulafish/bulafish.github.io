---
title: Nginx With Php-fpm Installation On CentOS7
author: bulafish
date: 2018-05-01 +0800
categories: [CentOS]
tags: [Installation]
---

Comparing [apache](https://httpd.apache.org/) and [nginx](https://nginx.org/en/), both have its strength and weakness.  Though apache still has higher market proportion, but it is undeniable that nginx is catching up and has the trend of increasing in market proportion from year to year.  Therefore, let's get to know more about nginx and how to use it.

[Php-fpm](https://php-fpm.org/), an alternative PHP [FastCGI](https://en.wikipedia.org/wiki/FastCGI) service, which is used by nginx to pass php web page requests to php.  Nginx itself is unable to interpret [php language](http://www.php.net/) written web pages, so when requests are sent from client to nginx, nginx send those requests to php with the help of php-fpm.  Then results are sent back to user with the help of php-fpm too through nginx from php after php is done interpreting.

So in this post, we will do
1. Install [epel](https://fedoraproject.org/wiki/EPEL), update the system.
2. Install nginx repository, install and start nginx.
3. Setup firewalld so nginx is accessible.
4. Install, setup and start php-fpm
5. Modify nginx to use php-fpm, restart service and check final output.

## Install epel and update system
Install epel package.
```bash
yum install http://mirror01.idc.hinet.net/EPEL/7/x86_64/Packages/e/epel-release-7-11.noarch.rpm
yum update -y
```
![nginx php-fpm](/assets/img/2018050101.jpg)  
![nginx php-fpm](/assets/img/2018050102.jpg)

## Install nginx repository, install and start nginx
First we check the version of nginx package provided by the official repository of CentOS and is `1.12.2-2.el7`, which is not too old compared to the nginx official release version.  I prefer to use the newest packages so I will start with adding nginx's official repository and do the rest of things.  
![nginx php-fpm](/assets/img/2018050103.jpg)

Add a new file inside `/etc/yum.repos.d/` directory, you can name the file anyway you like but .repo extension must be used.  Copy and paste the lines below into the file you created, save and exit.  Now check again the version of nginx package and will see that the newest version is `1.14.0-1.el7_4.ngx`.
```
[nginx]
name=nginx repo
baseurl=http://nginx.org/packages/centos/7/$basearch/
gpgcheck=0
enabled=1
```
![nginx php-fpm](/assets/img/2018050104.jpg)  
![nginx php-fpm](/assets/img/2018050105.jpg)

Install nginx.
```bash
yum install nginx -y
```
![nginx php-fpm](/assets/img/2018050106.jpg)

Now let's start nginx.  For more information about systemctl, please refer:
```bash
systemctl start nginx
systemctl status nginx
```
![nginx php-fpm](/assets/img/2018050115.jpg)

{% include ads3.html %}

## Allow http access to firewall or stop firewall
Stop firewall or allow http access to your server so we can test out nginx.  
For more information about firewall-cmd, please refer:
```bash
# To allow http access
firewall-cmd --add-service=http --permanent
firewall-cmd --reload
firewall-cmd --list-all

# To stop firewall service
systemctl stop firewalld
```
![nginx php-fpm](/assets/img/2018050107.jpg)

Now, browse to your server's ip and you will see nginx running!  
![nginx php-fpm](/assets/img/2018050116.jpg)

## Php-fpm installation and configuration
If we want to run web pages written in php, we must have php package installed.  Please install the version of php that you need.
```bash
yum install php
```
![nginx php-fpm](/assets/img/2018050108.jpg)

Install php-fpm.
```
yum install php-fpm
```
![nginx php-fpm](/assets/img/2018050109.jpg)

Start php-fpm service and check status.
```
systemctl start php-fpm
systemctl status php-fpm
```
![nginx php-fpm](/assets/img/2018050110.jpg)

Create a file named `index.php` with the content below at `/usr/share/nginx/html/`, which is  the default www directory of nginx for php-fpm testing later on.
```php
<?php
phpinfo();
?>
```
![nginx php-fpm](/assets/img/2018050111.jpg)

## Modify nginx to work with php-fpm
As mentioned before, nginx will pass php web pages request to php through the help of php-fpm, so let's modify the content of `/etc/nginx/conf.d/default.conf`.
1. Add index.php at line 10
2. Unmark line 30 ~ 36
3. Change line 31 to root /usr/share/nginx/html;
4. Change line 34 to $document_root$fastcgi_script_name;
5. Save, exit and restart nginx.

![nginx php-fpm](/assets/img/2018050113.jpg)
```
systemctl restart nginx
```
![nginx php-fpm](/assets/img/2018050114.jpg)

And finally, if you see the php infomation page, that means you succeed and we are done for this post!  
![nginx php-fpm](/assets/img/2018050112.jpg)