---
title: CentOS7 Systemctl Cheat Sheet
author: bulafish
date: 2018-04-27 +0800
categories: [CentOS]
tags: [Ops]
---

Since CentOS7, command `service` has been changed to `systemctl`.  For a while I was very repel to it cause I was new and not used to it at all.  Though that `service` is still usable in CentOS7 but after I thought, what the hell, why not learn and get used to something new?  Therefore, this article is produced as reference, for me and anyone else who is also struggling to learn and unfamiliar with `systemctl` like me.

Before we start, please note that systemctl not only can manage **service** but also **path**, **slice**, **snapshot**, **socket**, **swap**, **target** and **timer**.  And they are called `unit files`.

## Gathering systemd information
First of all, we can use help to list all the usages.
```bash
systemctl -h
```
![systemctl](/assets/img/2018042601.jpg)

It is important to know/list out all the installed unit files.
```bash
systemctl list-unit-files
```
![systemctl](/assets/img/2018042603.jpg)

`static` means it can't be start or stop like a `service`.  static unit files are mostly a "one time job" or is depended by other unit files.

Unit files name can be added for particular query.
```bash
systemctl list-unit-files sshd.service
```
![systemctl](/assets/img/2018042604.jpg)

Listing out the status of all the unit files.
```bash
systemctl list-units
```
![systemctl](/assets/img/2018042602.jpg)

Listing out the status of a particular unit file.
```bash
systemctl list-units sshd.service
```
![systemctl](/assets/img/2018042605.jpg)

Both systemctl list-units and systemctl list-unit-files can be appended with --type for particular type of unit file.
```bash
systemctl list-units --type=target
systemctl list-unit-files --type=path
```
![systemctl](/assets/img/2018042608.jpg)

Among all the unit file types, socket has it's own command to list out.
```bash
systemctl list-sockets
```
![systemctl](/assets/img/2018042609.jpg)

Show a unit's dependencies.
```bash
systemctl list-dependencies crond.service
```
![systemctl](/assets/img/2018042606.jpg)

{% include ads3.html %}

## Working with services
Commands to start, stop and showing status.
```bash
systemctl stop crond
systemctl status crond
systemctl start crond
```
![systemctl](/assets/img/2018042610.jpg)
<br>![systemctl](/assets/img/2018042611.jpg)

Commands to reload, restart.
```bash
systemctl reload crond
systemctl restart crond
```
![systemctl](/assets/img/2018042613.jpg)

Commands to enable and disable service at system start up.
```bash
systemctl disable crond
systemctl enable crond
```
![systemctl](/assets/img/2018042614.jpg)
<br>![systemctl](/assets/img/2018042615.jpg)

From the image we learn that to start or stop a service is actually just putting or removing a symlink of the unit file at `/etc/systemd/system/multi-user.target.wants/` directory.

A quick peep of `/etc/systemd/system/multi-user.target.wants/` directory.
<br>![systemctl](/assets/img/2018042619.jpg)

Checkout the current runlevel where `multi-user.target` equals to runlevel init 3.
```bash
systemctl get-default
```
![systemctl](/assets/img/2018042620.jpg)

Commands to change runlevel.
```bash
systemctl set-default graphical.target
```
![systemctl](/assets/img/2018042621.jpg)

We can tell from the message that the runlevel is also a symlink linked to default.target.
<br>![systemctl](/assets/img/2018042622.jpg)

## Viewing log messages
Lastly, command to view all service logs.
```bash
journalctl
```
![systemctl](/assets/img/2018042618.jpg)

To view log of particular service.
```bash
journalctl -u crond.services
```
![systemctl](/assets/img/2018042617.jpg)

To view the real time log.  The image shows the real time log pop out when I try to login the demo server.
```bash
journalctl -f
```
![systemctl](/assets/img/2018042616.jpg)

REFERENCES:
<br>[systemd Cheat Sheet for Red Hat Enterprise Linux 7 - Red Hat Customer Portal](https://access.redhat.com/articles/systemd-cheat-sheet)
