---
title: Change System Time On CentOS7
author: bulafish
date: 2018-05-06 +0800
categories: [CentOS]
tags: [Configuration]
---

Since the invention of virtual machine, it provides great convenience to the IT world.  But due to the nature of cloud service, the system time of the newly prevision virtual machine is pre-set and is usually set at UTC.  So in this post I am going to show you how to change system time zone to your desire choice and in this demo, CST will be used.

As usual, let's check the current time first, with the command `date`, we can see that the time zone is set at `UTC` now.  
![change system time](/assets/img/2018050601.png)

All the time zone files are pre-defined in the system already and located at `/usr/share/zoneinfo/`.  
![change system time](/assets/img/2018050602.png)

All we have to do is copy the desired time zone file to the appropriate system location and that is it!  Before we do that, let's rename the current time zone file first, just in case if anything goes wrong.  The current system time zone file is located at `/etc/localtime`.
```bash
mv /etc/localtime /etc/localtime.bak
```
![change system time](/assets/img/2018050603.png)

Then we can link or copy desired pre-defined system time zone file to `/etc/localtime`.  In this demo, I am just going to make a soft link.  After that, we issue `date` command again to confirm that the system time zone has been changed.
```bash
ln -sf /usr/share/zoneinfo/Asia/Taipei /etc/localtime
```
![change system time](/assets/img/2018050604.png)

{% include ads3.html %}

Next, to be safe, we can issue `ntpdate` command to synchronize system time with any of the public time server to the most accurate time.  In the image, we can see that the current system time is offset with the time servery by 0.000189 sec.
```bash
ntpdate pool.ntp.org
```
![change system time](/assets/img/2018050605.png)

And to make sure your system is always time synchronized, we can add the above command to `/etc/crontab` file and let it synchronizes time at the frequency you prefer.  `* * * * *` means execute the command follow by it every minute.
```bash
* * * * * root ntpdate pool.ntp.org
```
![change system time](/assets/img/2018050606.png)

To make sure `crontab` and the command added will be working correctly, let's use `date` command to set current system to `20180101 17:00:00`, wait for a minute and check if crontab has done its job as we expected.
```bash
date -s "20180101 17:00:00"
```
![change system time](/assets/img/2018050607.png)

And after a minute, the system date has been corrected as expected!  
![change system time](/assets/img/2018050608.png)