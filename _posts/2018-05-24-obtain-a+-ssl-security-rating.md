---
title: Obtain A+ SSL Security Rating
author: bulafish
date: 2018-05-24 +0800
categories: [CentOS]
tags: [Nginx, SSL]
---

In previous post, I have setup SSL with nginx on CentOS7 from letsencrypt.  Please refer: [Use Lets Encrypt Certbot to Install Free SSL on CentOS7](https://www.bulafish.com/centos/2018/05/21/use-lets-encrypt-certbot-to-install-free-ssl-on-centos7/).  
By default, the security setting of the SSL rating is `B`.  In this post, I will config necessary settings to obtain an A+ security rating.  
![ssl](/assets/images/2018052425.png)

Things to do to get A+.
1. generate a 4096 bits dhpara.pem
2. modify nginx.conf and demo.bulafish.com.conf

Extra things to do to increase security.
3. config [DNS CAA](https://en.wikipedia.org/wiki/DNS_Certification_Authority_Authorization)

Generate dhpara.pem, please change output location to fit your needs.  It takes a while of time.
```bash
openssl dhparam -out /etc/letsencrypt/live/demo.bulafish.com/dhparam.pem 4096
```
![ssl](/assets/images/2018052424.png)

{% include ads3.html %}

Modify `/etc/nginx/nginx.conf`, add the following code inside http block, save and exit.
```nginx
ssl_dhparam /etc/letsencrypt/live/demo.bulafish.com/dhparam.pem;
ssl_protocols TLSv1 TLSv1.1 TLSv1.2;
ssl_prefer_server_ciphers on;
ssl_ciphers ECDHE-RSA-AES256-GCM-SHA512:DHE-RSA-AES256-GCM-SHA512:ECDHE-RSA-AES256-GCM-SHA384:DHE-RSA-AES256-GCM-SHA384:ECDHE-RSA-AES256-SHA384;
ssl_session_timeout  10m;
ssl_session_cache shared:SSL:10m;
ssl_session_tickets off;
add_header Strict-Transport-Security "max-age=63072000; includeSubDomains; preload";
add_header X-Frame-Options DENY;
add_header X-Content-Type-Options nosniff;
add_header X-XSS-Protection "1; mode=block";
add_header X-Robots-Tag none;
server_tokens off;

ssl_stapling on;
ssl_stapling_verify on;
resolver 8.8.8.8 8.8.4.4 valid=300s;
resolver_timeout 5s;
```
![ssl](/assets/images/2018052427.png)

Modify `your_server.conf`, add the following code inside server block, save, exit and restart nginx.
```nginx
ssl_certificate     /etc/letsencrypt/live/demo.bulafish.com/fullchain.pem;
ssl_certificate_key /etc/letsencrypt/live/demo.bulafish.com/privkey.pem;
ssl_trusted_certificate /etc/letsencrypt/live/demo.bulafish.com/chain.pem;
```
![ssl](/assets/images/2018052426.png)

Re-run the score test again to confirm an A+ rating is obtained.  
![ssl](/assets/images/2018052428.png)

[What is a CAA record?](https://support.dnsimple.com/articles/caa-record/)
>A Certification Authority Authorization (CAA) record is used to specify which certificate authorities (CAs) are allowed to issue certificates for a domain.

To configure CAA, login to your dns server and add a CAA record.  
![ssl](/assets/images/2018052429.png)  
![ssl](/assets/images/2018052430.png)

Wait for couple hours to let the CAA record take effect, run the rating test again to confirm if CAA policy is working.  
![ssl](/assets/images/2018052431.png)
