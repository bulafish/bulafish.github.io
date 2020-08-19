---
title: Nginx and PHP-FPM File System Permission
author: bulafish
date: 2018-08-10 +0800
categories: [CentOS]
tags: [Nginx, Configuration]
---

When Nginx is using socket to communicate with PHP-FPM, in the PHP-FPM configuration file, the section below must be configured.
Assuming that Nginx is running with `user nginx`, the socket file must be configured with the appropriate permission, as in the configuration file.  So Nginx has the right permission to communicate with PHP-FPM through socket file.
```
; Set permissions for unix socket, if one is used. In Linux, read/write
; permissions must be set in order to allow connections from a web server. Many
; BSD-derived systems allow connections regardless of permissions.
; Default Values: user and group are set as the running user
;                 mode is set to 0666
listen.owner = nginx
listen.group = nginx
;listen.mode = 0666
```

Since all the php file requests are redirected to PHP-FPM, so the file system must be set with a permission where PHP-FPM has the privileges to access them, specially write permission.  The section below sets the `user identity` to PHP-FPM to use when accessing the system files.  In this case, PHP-FPM is using `nginx` to access system files, so `nginx` should be set to the file permissions.

{% include ads3.html %}

```
; Unix user/group of processes
; Note: The user is mandatory. If the group is not set, the default user's group
;       will be used.
; RPM: apache Choosed to be able to access some dir as httpd
user = nginx
; RPM: Keep a group allowed to write in log dir.
group = nginx
```

The default setting for user and group above is apache.  In some cases, the file system would set to `nginx`, but since PHP-FPM is configured by default to access file system with identity `apahce`, so permission denied issue would occur.  The problem can be solved by changing user and group from `apache` to `nginx` and restart PHP-FPM service OR add `user apache` into `nginx group` and change the permission to 775.

Command to add apache into nginx's group  
* -a = append  
* -G = supplementary group  
* nginx = add to this group  
* apache = user to add

```
usermod -a -G nginx apache
chmod 775 /your/system/file/location
```

PHP-FPM must be restarted so the change of permission would take effect.
