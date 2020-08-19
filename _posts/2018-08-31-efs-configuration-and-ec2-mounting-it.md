---
title: EFS Configuration with EC2 Mounting It
author: bulafish
date: 2018-08-31 +0800
categories: [AWS]
tags: [EC2, EFS]
---

### Introduction
Steps to create EFS from console and mount EFS in EC2, main focus will be on EFS


### Create security group for EFS
  * EFS is using NFS version 4.0 and 4.1, so we must create a security group to allow tcp 2049 port accessible to EFS
  * When creating security group for EFS, we can select the source allow to access EFS which the security group for EC2 will be selected  
  ![efs](/assets/images/2018083101.png)

  * Finished SG view of EFS  
  ![efs](/assets/images/2018083102.png)


### Create EFS
  * Make sure the correct VPC, subnet and SG is selected  
  ![efs](/assets/images/2018083103.png)

  * Remove the default SG and select the EFS SG  
  ![efs](/assets/images/2018083104.png)

  * Rest of steps to complete EFS, configure accordingly  
  ![efs](/assets/images/2018083105.png)  
  ![efs](/assets/images/2018083106.png)

  * Finshed view of EFS configuring  
  ![efs](/assets/images/2018083107.png)

  {% include ads3.html %}

  * EFS creating and finished  
  ![efs](/assets/images/2018083108.png)  
  ![efs](/assets/images/2018083111.png)


### Steps to mount EFS to EC2
  * Follow the provided instructions
  * Install necessary package  
  ![efs](/assets/images/2018083109.png)  

  * Follow the instruction to mount the EFS mounting target  
  ![efs](/assets/images/2018083110.png)


### Login to EC2 and mount EFS mounting target
  * Install necessary package  
  ![efs](/assets/images/2018083112.png)

  * Mount EFS mounting target  
  ![efs](/assets/images/2018083113.png)


### Mount EFS mounting target automatically after reboot
  * Modify `/etc/fstab` as bellow, reboot to confirm
  ```bash
  sudo sh -c "cat >> /etc/fstab"
  your_EFS_file_system_id mount_point efs defaults,_netdev 0 0
  ```
  ![efs](/assets/images/2018083117.png)

  * EFS auto mounted after reboot  
  ![efs](/assets/images/2018083118.png)


### Checking EFS statistics with CloudWatch
  * test and monitor EFS with command bloew, replace the `mounting point`
  ```bash
  sudo fio --name=fio-efs --filesize=10G --filename=/`ur_mount_point`/fio-efs-test.img --bs=1M --nrfiles=1 --direct=1 --sync=0 --rw=write --iodepth=200 --ioengine=libaio
  ```
  ![efs](/assets/images/2018083122.png)

  * Enter CloudWatch console  
  ![efs](/assets/images/2018083119.png)

  * Select metrics to be monitor  
  ![efs](/assets/images/2018083120.png)

  * Set configuration according to needs and check the result graphp above  
  ![efs](/assets/images/2018083121.png)


### Finished view of testing command  
  ![efs](/assets/images/2018083123.png)
