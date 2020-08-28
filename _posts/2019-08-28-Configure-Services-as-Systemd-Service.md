---
title: Configure Services as Systemd Service
author: bulafish
date: 2019-08-28 +0800
categories: [CentOS]
tags: [Configuration]
---

<br>

## Goal：
Configure services to be managed by Systemd using systemctl command

## briefting：
Explanation from man systemctl
> systemctl may be used to introspect and control the state of the "systemd" system and service manager.  Please refer to systemd(1) for an introduction into the basic concepts and functionality this tool manages.

Location of configuration files is at `/user/lib/systemd/system`
![systemctl](/assets/img/20200828034.png)

Where when enable the service through `systemctl` command, a soft link will be created at `/etc/systemd/system/multi-user.target.wants`
![systemctl](/assets/img/20200828035.png)

The purpose of this post is to record down the systemd configuration files made for Prometheus, alertmanager, nginx-exporter etc

`***`

## Prometheus

```shell
[Unit]
Description=prometheus
After=network.target

[Service]
Type=simple
#ExecStart=/opt/prometheus/prometheus --web.enable-admin-api --config.file=/opt/prometheus/prometheus.yml
ExecStart=/opt/prometheus/prometheus --config.file=/opt/prometheus/prometheus.yml
ExecReload=/bin/kill -s HUP $MAINPID
ExecStop=/bin/kill -s TERM $MAINPID
Restart=on-failure

[Install]
WantedBy=multi-user.target
```

{% include ads3.html %}

## Nginx Exporter

```shell
[Unit]
Description=NGINX Prometheus Exporter
After=network.target

[Service]
Type=simple
ExecStart=/opt/nginx-prometheus-exporter
Restart=always

[Install]
WantedBy=multi-user.target
```

## Alertmanager

```shell
[Unit]
Description=alertmanager
After=network.target

[Service]
Type=simple
ExecStart=/opt/alertmanager/alertmanager --config.file=/opt/alertmanager/alertmanager.yml
ExecReload=/bin/kill -s HUP $MAINPID
ExecStop=/bin/kill -s TERM $MAINPID
Restart=on-failure

[Install]
WantedBy=multi-user.target
```