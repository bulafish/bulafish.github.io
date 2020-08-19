---
title: SSH Authentication Refused Bad Ownership or Modes for Directory
author: bulafish
date: 2018-04-26 +0800
categories: [CentOS]
tags: [Configuration]
---

For convenience, I use public/private key ssh authentication to login remote servers.  All servers work out fine but just this one particular server did not work out.  It has been bothering me for a while and finally I got fed up today and decided to solve it once for all.  If you have the same problem, please check below for description and steps for solution!

## Description
The content of `/var/log/secure` from remote server actually points out cause of the problem already.
<br>![SSH Authentication Refused Bad Ownership or Modes for Directory](/assets/img/2018042601.png)
<br>![SSH Authentication Refused Bad Ownership or Modes for Directory](/assets/img/2018042602.png)

{% include ads3.html %}

We can see clearly that the permission of the directories is wrongly set, so all we have to do is set it correctly!  
The main point to get all these things work is **NOT** to have `write` permission set to the `group`!  
According to [CentOS wiki](https://wiki.centos.org/HowTos/Network/SecuringSSH#head-9c5717fe7f9bb26332c9d67571200f8c1e4324bc), 700 is the preferred permission to the directory but after testing, it will be fine as long as write permission is not set to group.

## Solution steps
Assume permission of my directories are set as below
<br>![SSH Authentication Refused Bad Ownership or Modes for Directory](/assets/img/2018042603.png)

Use commands below to set direcotory's permission correctly.
``` bash
chmod 700 ~/.ssh
chmod 700 ~/
```

<br>![SSH Authentication Refused Bad Ownership or Modes for Directory](/assets/img/2018042604.png)

This solved my problem, does it solve yours?

REFERENCES:
<br>[HowTos/Network/SecuringSSH - CentOS Wiki](https://wiki.centos.org/HowTos/Network/SecuringSSH)
<br>[SSH Authentication refused: bad ownership or modes for directory - Dave Perrett](https://www.daveperrett.com/articles/2010/09/14/ssh-authentication-refused/)
