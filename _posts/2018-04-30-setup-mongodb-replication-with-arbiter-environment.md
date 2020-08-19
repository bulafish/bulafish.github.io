---
title: Setup MongoDB Replication With Arbiter Environment
author: bulafish
date: 2018-04-30 +0800
categories: [Database]
tags: [Mongodb]
---

An official explanation of what is replica set in MongoDB.
>A replica set in MongoDB is a group of mongod processes that maintain the same data set. Replica sets provide redundancy and high availability, and are the basis for all production deployments.

>Replication provides redundancy and increases data availability. With multiple copies of data on different database servers, replication provides a level of fault tolerance against the loss of a single database server.

In this post, for simplicity, I am going to setup a mongodb replica set environment like the image below.  
![mongodb replica](/assets/img/replica-set-primary-with-secondary-and-arbiter.bakedsvg.svg)

[Arbiter](https://docs.mongodb.com/manual/core/replica-set-arbiter/) is a special role in this environment, it does not maintain any data but to maintain a quorum in a replica set by responding to hearteat and election rquests by other replica set members.

Steps for this tutorial,
1. Prepare three servers with mongodb installed.  Make sure each server is able to coonect to every other servers through 27017 port.  Please refer: [MongoDB-3.6 Community Edition Installation](https://www.bulafish.com/db/2018/04/30/mongodb-community-edition-installation/)
2. Create necessary users and enable authentication.  Please refer: [MongoDB Enable User Login Authentication](https://www.bulafish.com/db/2018/04/30/mongodb-enable-authentication/)
3. Create [Keyfile Security](https://docs.mongodb.com/manual/tutorial/enforce-keyfile-access-control-in-existing-replica-set/#keyfile-security) for replica set internal authentication access control.
4. Modify `/etc/mongod.conf` to the use of replication set.
5. Configure and start replica set service, check result.
6. Add arbiter and check result.

Assuming you have done steps 1 & 2, my servers information is below.

name|ip
-|-
mongo1  |  192.168.122.134
mongo2  |  192.168.122.168
Arbiter |  192.168.122.215

![mongodb replica](/assets/img/2018050801.png)

## Create Keyfile Security

Create `keyfile` on `mongo1` which is used by all the mongod instances within the relica set for authentication.  You must save keyfile at a location where mongod has read privilege.  Please run the commands with root privilege.
```bash
openssl rand -base64 756 > /var/lib/mongo/mongodb-keyfile
chmod 400 /var/lib/mongo/mongodb-keyfile
chown mongod.mongod /var/lib/mongo/mongodb-keyfile
```
![mongodb replica](/assets/img/2018043029.png)

Next, copy the keyfile to `mongo2` and `arbiter`.  Make sure mongod on both server has read privilege of the file.  
![mongodb replica](/assets/img/2018050810.png)

## Modify mongod.conf to the use of replication

We need to modify `/etc/mongod.conf` of `mongo1`, `mongo2` and `arbiter` so that it will authenticate with keyfile and also set a name for replica set.  Edit bindIp in according to your server ip.
```bash
# network interfaces
net:
  port: 27017
  bindIp: 192.168.122.134  # change ip to mongo2 and arbiter accordingly.

security:
  keyFile: /var/lib/mongo/mongodb-keyfile

replication:
  replSetName: bulafish
```

![mongodb replica](/assets/img/2018050811.png)

{% include ads3.html %}

Now start `mongo1`, `mongo2` and `arbiter`.
```bash
systemctl start mongod
```
![mongodb replica](/assets/img/2018043031.png)
![mongodb replica](/assets/img/2018043032.png)
![mongodb replica](/assets/img/2018050812.png)

## Configure, start replica set and check result

Next we need to initial and conf the replica set, let's login to `mongo1` using syntax `mongo --host serverip -u username -p --authenticationDatabase admin`.
```mongodb
mongo --host 192.168.122.134 -u blogroot -p --authenticationDatabase admin
```
![mongodb replica](/assets/img/2018050802.png)

Initial replica set.
```mongodb
rs.initiate()
```
![mongodb replica](/assets/img/2018050803.png)

Now we config the replica set.  At this stage, we can see that `bulafish` replica set has been configured.
```mongodb
rs.conf()
```
![mongodb replica](/assets/img/2018050804.png)

Now we add `mongo2` into replica set.
```mongodb
rs.add("192.168.122.168")
```
![mongodb replica](/assets/img/2018050805.png)

Check the current status of replica set.  We will see that `mongo1` and `mongo2` are in the `members` section with mongo1 showing `primary` and mongo2 showing `secondary`.
```mongodb
rs.status()
```
![mongodb replica](/assets/img/2018050806.png)  
![mongodb replica](/assets/img/2018050807.png)

## Add arbiter into replica set

Use `rs.addArb()` to add an arbiter into relica set.
```mongodb
rs.addArb("192.168.122.215")
```
![mongodb replica](/assets/img/2018050808.png)

Check the status after arbiter has been added.  
![mongodb replica](/assets/img/2018050809.png)

That's it, we have deployed an replica set environment with an arbiter!

REPLATED POSTS:  
[MongoDB-3.6 Community Edition Installation](https://www.bulafish.com/db/2018/04/30/mongodb-community-edition-installation/)  
[MongoDB Enable User Login Authentication](https://www.bulafish.com/db/2018/04/30/mongodb-enable-authentication/)

REFERENCES:  
[Deploy a Replica Set](https://docs.mongodb.com/manual/tutorial/deploy-replica-set/)  
[MongoDB Replica Set 高可用性架構搭建](https://blog.toright.com/posts/4508/mongodb-replica-set-%E9%AB%98%E5%8F%AF%E7%94%A8%E6%80%A7%E6%9E%B6%E6%A7%8B%E6%90%AD%E5%BB%BA.html)
