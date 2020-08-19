---
title: Use Lets Encrypt Certbot to Install Free SSL on CentOS7
author: bulafish
date: 2018-05-21 +0800
categories: [CentOS]
tags: [Installation, Nginx, SSL]
---

Since google starts promoting SSL, SSL has become commonly use nowadays and because of this, many companies start to provide free ssl, which [Let's Encrypt](https://letsencrypt.org/) is one of the earliest free ssl service provider.  Let's Encrypt provides a bot called [certbot](https://certbot.eff.org/) which much ease the process of applying, configuring, renewing and maintaining the SSLs compare to the early times.  In this post, I am going to show you how to use certbot to apply and set renew for the free ssl certificate with nginx.

Choose your web service and OS.  
![letsencrypt](/assets/images/2018052111.png)

Certbot package is provided by the EPEL repository, make sure it is installed.
```bash
yum install epel-release
```

Install certbot for nginx.
```bash
install certbot-nginx
```
![letsencrypt](/assets/images/2018052112.png)

Use certbot to obain ssl and modify nginx config file automatically.
>Running this command will get a certificate for you and have Certbot edit your Nginx configuration automatically to serve it. If you're feeling more conservative and would like to make the changes to your Nginx configuration by hand, you can use the certonly subcommand:

```bash
certbot --nginx
```

To obtain ssl only, make sure your http(80) port is public accessible.
```bash
certbot --nginx certonly --rsa-key-size 4096
```
![letsencrypt](/assets/images/2018052113.png)  
![letsencrypt](/assets/images/2018052114.png)

SSL certificate obtained.  
![letsencrypt](/assets/images/2018052115.png)

{% include ads3.html %}

Now modify nginx config file to enable ssl protocol.  Save, exit and restart nginx service.
```nginx
listen 80;
listen 443 ssl;
server_name  demo.bulafish.com;

ssl_certificate     /etc/letsencrypt/live/demo.bulafish.com/fullchain.pem;
ssl_certificate_key /etc/letsencrypt/live/demo.bulafish.com/privkey.pem;
```
![letsencrypt](/assets/images/2018052121.png)

Check if ssl works correctly.  
![letsencrypt](/assets/images/2018052116.png)

Modify nginx config file to use nginx rewrite to force / redirect http to https.  Make http an individual server block and use return to redirect to https.
```nginx
server {
    listen       80;
    server_name  demo.bulafish.com;
    return 301 https://$server_name$request_uri;
}
```

Make https a new individual server block.
```nginx
server {
    listen 443 ssl;
    server_name  demo.bulafish.com;
```
![letsencrypt](/assets/images/2018052119.png)


Try certbot auto renew before setting up.
>Certbot can be configured to renew your certificates automatically before they expire. Since Let's Encrypt certificates last for 90 days, it's highly advisable to take advantage of this feature. You can test automatic renewal for your certificates by running this command:

```bash
certbot renew --dry-run
```
![letsencrypt](/assets/images/2018052117.png)

If everything works out fine, then we can add a cron job to run it twice a day at 3AM and 11AM.
```bash
0 3,11 * * * /bin/certbot renew
```
![letsencrypt](/assets/images/2018052118.png)
