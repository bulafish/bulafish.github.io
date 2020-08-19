---
title: CentOS7 firewall-cmd Cheat Sheet
author: bulafish
date: 2018-04-27 +0800
categories: [CentOS]
tags: [Ops]
---

Since CentOS7, same as `service` and `systemctl`, `iptables` has been changed to `firewall-cmd`.  
For more information about `systemclt`, please check here: [CentOS7 Systemctl Cheat Sheet](https://www.bulafish.com/centos/2018/04/27/centos7-systemctl-cheat-sheet/)  
Please note that iptables and firewalld cannot run at the same time.  If both services are installed, one will stop automatically when the other one is started.  You can install `iptables-services` package to use iptables service.

Since CentOS7, firewalld use the idea of `zones` and `services`.  Each zone can have its own  firewall setting rules.  When server is configured to particular zone, the rules set inside the zone apply to the server network.  Services are equal to the ports that you want to allow or deny.

There are nine default zones set by CentOS7, which are
1. public
2. external
3. dmz
4. work
5. home
6. internal
7. trusted
8. drop
9. block

For more information about each zone, please refer: [Documentation - Zone - Predefined Zones \| firewalld](http://www.firewalld.org/documentation/zone/predefined-zones.html)

As usual, we begin with `-h` and it will list out many many many many options!!
```bash
firewall-cmd -h
```
![firewall-cmd](/assets/img/2018042710.png)

Now it is important to know all the zones available in our server, the default zone and the zone it is using right now.
```bash
firewall-cmd --get-zones
firewall-cmd --get-default-zone
firewall-cmd --get-active-zone
```
![firewall-cmd](/assets/img/2018042711.png)  
we can see that right now our server is using the zone `public`.

Now list out all the predefined services.
```bash
firewall-cmd --get-services
```
![firewall-cmd](/assets/img/2018042712.png)

Check out current status of firewall rule.
```bash
firewall-cmd --list-all
```
![firewall-cmd](/assets/img/2018042721.png)  
We can see that right now there are two services that is set open/allow to all, which are ssh and dhcpv6-client.

Let's add http service to firewall rule.  Before we start, let's check the status first.  My destination server ip is 192.168.122.206, http service is not set in the firewall rule, I can see that 80 port is running on the server and I can access to 80 port within `blogdemo`.  
But from my source server, `coffee`, accessing to `blogdemo's` 80 port is failed.  
![firewall-cmd](/assets/img/2018042713.png)
![firewall-cmd](/assets/img/2018042714.png)

{% include ads3.html %}

Now add http service to `blogdemo's` firewall rule. `--zone=public` is not mandatory but it is also good to have command as specific as possible.
```bash
firewall-cmd --add-service=http --zone=public
```
![firewall-cmd](/assets/img/2018042715.png)

Now try to access `blogdemo's` 80 port again from `coffee`.  
![firewall-cmd](/assets/img/2018042716.png)

Now if we reload firewalld, we will find that http is remove from service.  That's because when we add http service, it was loaded into the memory but when we reload it, it actually read all the configure from a file located at `/etc/firewalld/zones/public.xml`.  Which work just the same way as iptables.
```bash
firewall-cmd --reload
```
![firewall-cmd](/assets/img/2018042717.png)

Therefore if we want to add a service permanently, we append --permanent at the end of the command and the change is write into the .xml file directly, then we have to reload the service right away so the changes will take effect from .xml file into memory.  Steps are demonstrates at the image below.
```bash
firewall-cmd --add-service=http --zone=public --permanent
```
![firewall-cmd](/assets/img/2018042718.png)

Next we are going to add particular ip with particular service to the rule AND particular ip to all services.  For achieving this, we use `--add-rich-rule`.  Do not forget to reload so the rules will take effect.  For checking rich rules, we can use `--list-rich-rules`.
```bash
firewall-cmd --add-source=192.168.122.1/32 --zone=public --permanent
firewall-cmd --add-rich-rule='rule family=ipv4 source address=192.168.122.0/24 service name=ssh accept' --permanent
firewall-cmd --list-rich-rules
```
![firewall-cmd](/assets/img/2018042719.png)

For removing rules, just simply change `add` to `remove`.  Again, don't forget to reload.
```bash
firewall-cmd --remove-service=http --zone=public
firewall-cmd --remove-source=192.168.122.1/32 --zone=public --permanent
firewall-cmd --remove-rich-rule='rule family=ipv4 source address=192.168.122.0/24 service name=ssh accept' --permanent
```
![firewall-cmd](/assets/img/2018042720.png)

REFERENCES:  
[Home | firewalld](http://www.firewalld.org/)  
[CentOS 7 Firewalld 防火牆說明介紹 @ 黃昏的甘蔗 :: 隨意窩 Xuite日誌](http://blog.xuite.net/tolarku/blog/363801991-CentOS+7+Firewalld+%E9%98%B2%E7%81%AB%E7%89%86%E8%AA%AA%E6%98%8E%E4%BB%8B%E7%B4%B9)  
[How To Use Firewalld Rich Rules And Zones For Filtering And NAT](https://www.rootusers.com/how-to-use-firewalld-rich-rules-and-zones-for-filtering-and-nat/)  
[Useful ‘FirewallD’ Rules to Configure and Manage Firewall in Linux](https://www.tecmint.com/firewalld-rules-for-centos-7/2/)  
[Whitelist source IP addresses in CentOS 7](https://unix.stackexchange.com/questions/159873/whitelist-source-ip-addresses-in-centos-7?utm_medium=organic&utm_source=google_rich_qa&utm_campaign=google_rich_qa)
