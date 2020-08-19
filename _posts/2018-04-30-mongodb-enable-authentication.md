---
title: MongoDB Enable User Login Authentication
author: bulafish
date: 2018-04-30 +0800
categories: [Database]
tags: [Mongodb]
---

In the previous post, I have installed Mongodb-3.6 Community Edition, please refer here:  
[MongoDB-3.6 Community Edition Installation](https://www.bulafish.com/db/2018/04/30/mongodb-community-edition-installation/).  
By default, mongodb starts with no authentication required, meaning that anyone can login to your mongodb.  Therefore in this post, I am going to write about how to add user rights and enable authentication.

Firs of all, make sure mongod is running.
```bash
ps aux | grep mongod
```
![mongodb authentication](/assets/img/2018043012.png)

We can see clearly that mongod is using the config file from `/etc/mongod.conf`, so we will modify that file later on.  Let's briefly talk about how mongodb authentication works.  Mongodb uses `role` to define/set users with the level and/or power of what an user can do.  For all the built-in roles available, please refer: [Built-In Roles &mdash; MongoDB Manual 3.6](https://docs.mongodb.com/manual/reference/built-in-roles/#userAdminAnyDatabase).

And according to the prerequisites, we must
1. Use [localhost exception](https://docs.mongodb.com/manual/core/security-users/#localhost-exception) to login mongod.
2. Create our first user in `admin` database granted with [userAdmin](https://docs.mongodb.com/manual/reference/built-in-roles/#userAdmin) or [userAdminAnyDatabase](https://docs.mongodb.com/manual/reference/built-in-roles/#userAdminAnyDatabase) built-in role.
3. Create other users with user created in step 2.

![mongodb authentication](/assets/img/2018043024.png)

Let's use localhost exception to login mongod.
```bash
mongo
```
![mongodb authentication](/assets/img/2018043014.png)

At login, mongodb prompts warnings suggesting for better configurations.  So let's fix it first.  Before we start, check the current status first.
<br>![mongodb authentication](/assets/img/2018043015.png)

Use the commands to fix the problem.  First two lines will change the current status, last two line will add the command to a script where it will be executed every time server reboots.  Please note that the following commands must be run by root privileges.  Please run `chmod +x /etc/rc.d/rc.local` to enable rc.local.
```bash
echo never > /sys/kernel/mm/transparent_hugepage/enabled
echo never > /sys/kernel/mm/transparent_hugepage/defrag
echo "echo never > /sys/kernel/mm/transparent_hugepage/enabled" >> /etc/rc.d/rc.local
echo "echo never > /sys/kernel/mm/transparent_hugepage/defrag" >> /etc/rc.d/rc.local
```
![mongodb authentication](/assets/img/2018043016.png)

{% include ads3.html %}

Restart mongod and login again, the warnings are gone.
```bash
systemctl restart mongod
mongo
```
![mongodb authentication](/assets/img/2018043017.png)

According to mongodb document, let's create our first user.
```
use admin
db.createUser(
  {
    user: "blogadmin",
    pwd: "12345678",
    roles: [
       { role: "userAdminAnyDatabase", db: "admin" },       
    ]
  }
)
```
![mongodb authentication](/assets/img/2018043018.png)

Now we have created the first user, we can use the same routine to create users for operation needed.  For testing purpose, I am just going to create a `root` user whom has the full privileges to the db.
```
use admin
db.createUser(
  {
    user: "blogroot",
    pwd: "12345678",
    roles: [
       { role: "root", db: "admin" },       
    ]
  }
)
```
![mongodb authentication](/assets/img/2018050702.png)

Now let's enable the authentication mechanism by editing mongodb config file as below.
<br>![mongodb authentication](/assets/img/2018043019.png)

Save, exit and restart the service.
```bash
vim /etc/mongod.conf
systemctl restart mongod
```
![mongodb authentication](/assets/img/2018043020.png)

Now let's try login again by just using `mongo` and trying to list all dbs.
<br>![mongodb authentication](/assets/img/2018043021.png)

We can see that errmsg displays "not authorized on admin to execute command", that means our setting works.  Now let's use authorized user to login.  There are two ways to do so, one is using this syntax `mongo -u username -p --authenticationDatabase admin`.
<br>![mongodb authentication](/assets/img/2018043022.png)

Another way is to use [db.auth()](https://docs.mongodb.com/manual/reference/method/db.auth/#db.auth) function provided by mongodb.  To use it, we first login with `mongo`.
<br>![mongodb authentication](/assets/img/2018043023.png)

RELATED POSTS:  
[MongoDB-3.6 Community Edition Installation](https://www.bulafish.com/db/2018/04/30/mongodb-community-edition-installation/)  
[Setup MongoDB Replication With Arbiter Environment](https://www.bulafish.com/db/2018/04/30/setup-mongodb-replication-with-arbiter-environment/)

REFERENCES:  
[Users &mdash; MongoDB Manual 3.6](https://docs.mongodb.com/manual/core/security-users/)  
[Authentication &mdash; MongoDB Manual 3.6](https://docs.mongodb.com/manual/core/authentication/)
