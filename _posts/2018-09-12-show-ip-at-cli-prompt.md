---
title: Show IP at CLI Prompt
author: bulafish
date: 2018-09-12 +0800
categories: [CentOS]
tags: [Configuration]
---

The initial request was to show private server IP on CLI prompt and derived to apply on server with public IP.  

The steps for server with private IP are:
1. Use command `hostname` to get server's private IP
2. Put the command above in bashrc file, so it will refresh every time login
3. Modify the layout of CLI prompt
4. Done

The steps for server with public IP are:
1. Get public IP from config.me with command `curl`
2. Save the result above in env variable
3. Put commands above in a shell script
4. Put the shell script above in rc.local file so it will refresh on every reboot (you can put it on bashrc too)
5. Modify the layout of CLI prompt
6. Done


### For server with private IP  
Get server IP
```bash
hostname -I
```
![sed](/assets/images/2018091202.png)

Modify bashrc file, save returned IP to variable ip
```bash
ip=`hostname -I | sed -e 's/[[:space:]]*$//'`
```
![sed](/assets/images/2018091204.png)  
![sed](/assets/images/2018091205.png)

Modify layout of CLI prompt, please refer below  
[Set Or Change Hostname In CentOS7](https://www.bulafish.com/centos/2018/04/24/set-or-change-hostname-in-centos7/)  
[Change Command Line Prompt Color In CentOS7](https://www.bulafish.com/centos/2018/04/25/change-command-line-color-in-centos7/)

Completed view of bashrc file
```bash
PS1='\[\e[01;36m\]\u\[\e[01;37m\]@\[\e[01;33m\]\H\[\e[01;37m\]-\[\e[01;35m\]'"$ip"'\[\e[01;37m\]:\[\e[01;32m\]\w\[\e[01;37m\]\$\[\033[0;37m\] '
```
![sed](/assets/images/2018091206.png)

Re-login to confirm the result.  Before and after  
![sed](/assets/images/2018091207.png)

{% include ads3.html %}

### For server with public IP  
Get server public IP
```bash
curl -s ifconfig.me
```
![sed](/assets/images/2018091208.png)

Create variable in environment for future replacement
```bash
echo "ip=1.2.3.4" >> /etc/environment  
```
![sed](/assets/images/2018091209.png)

Save public IP to env variable and save as shell script
```bash
ip=$(curl -s ifconfig.me)
sed -i -e "s/ip=.*/ip=$ip/g" /etc/environment
```
![sed](/assets/images/2018091210.png)

Add getit.sh to rc.local file to execute on every reboot
```bash
echo "/bin/bash /root/getip.sh" >> /etc/rc.d/rc.local
```
![sed](/assets/images/2018091211.png)

Make files executable
```bash
chmod +x /etc/rc.d/rc.local /root/getip.sh
```
![sed](/assets/images/2018091212.png)

Modify bashrc
```bash
PS1='\[\e[01;36m\]\u\[\e[01;37m\]@\[\e[01;33m\]\H\[\e[01;37m\]-\[\e[01;35m\]'"$ip"'\[\e[01;37m\]:\[\e[01;32m\]\w\[\e[01;37m\]\$\[\033[0;37m\] '
```
![sed](/assets/images/2018091213.png)

Reboot server to confirm result  
![sed](/assets/images/2018091214.png)
