---
title: Configure Supervisor
author: bulafish
date: 2019-08-28 +0800
categories: [CentOS]
tags: [Configuration]
---

<br>

## Goal：
Use [Supervisor](http://supervisord.org/introduction.html) as guardian angel to monitor process running on Linux, restart it if it fails

## briefting：
Explanation from official website
> Supervisor is a client/server system that allows its users to control a number of processes on UNIX-like operating systems.
Configuration file of Supervisor is located at `/etc/supervisord.conf`
Configuration file of monitored services can be put at `/etc/supervisord.d/`
![systemctl](/assets/img/20200828036.png)

The purpose of this post is to record down the configuration files made for node-exporter

`***`

{% include ads3.html %}

## node-exporter

```shell
[program:node_exporter]
command=/opt/node_exporter/node_exporter
autostart=true
autorestart=true
startretries=3
stderr_logfile=/var/log/supervisor/node_exporter.err.log
stdout_logfile=/var/log/supervisor/node_exporter.out.log
```

Use systemctl command to start supervisord service.  Once started, node_exporter service is started as well.