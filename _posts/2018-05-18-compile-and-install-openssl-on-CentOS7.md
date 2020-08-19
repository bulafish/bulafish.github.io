---
title: Compile and Install OPENSSL-1.1.0 on CentOS7
author: bulafish
date: 2018-05-18 +0800
categories: [CentOS]
tags: [Installation]
---

>The latest stable version is the 1.1.0 series. The 1.0.2 series is our Long Term Support (LTS) release, supported until 31st December 2019. The 0.9.8, 1.0.0 and 1.0.1 versions are now out of support and should not be used.

Check the current openssl version.
```bash
openssl version
```
![openssl](/assets/images/2018051805.png)

Download openssl tarball file from official website.
```bash
wget https://www.openssl.org/source/openssl-1.1.0h.tar.gz
```
![openssl](/assets/images/2018051806.png)

Decompress and configure the package.
```bash
tar -zxf openssl-1.1.0h.tar.gz
cd openssl-1.1.0h.tar.gz
./config
```
![openssl](/assets/images/2018051807.png)

{% include ads3.html %}

If everything goes well, you will see image blow, if not, please follow the instruction returned to solve the problems.  
![openssl](/assets/images/2018051808.png)

Make and install package.  If you are using user privileges, make sure to use sudo.
```bash
make && make install
```
![openssl](/assets/images/2018051809.png)

If everything goes well, the process will end without error msg.  
![openssl](/assets/images/2018051810.png)

The new openssl binary file is placed at `/usr/local/bin`, so right now, there are two openssl binary files exist in the system.  
![openssl](/assets/images/2018051811.png)

But the `current connection` is still using the old version of openssl, even though `which` command shows the new location of openssl.  
![openssl](/assets/images/2018051812.png)

All we have to do is re-login the server to fresh the connection.  
![openssl](/assets/images/2018051813.png)

REFERENCES:  
[Openssl-Download Page](https://www.openssl.org/source/)
