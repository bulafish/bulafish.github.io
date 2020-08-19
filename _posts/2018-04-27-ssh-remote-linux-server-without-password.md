---
title: SSH Remote Linux Server Without Password
author: bulafish
date: 2018-04-27 +0800
categories: [CentOS]
tags: [Configuration]
---

Couple days ago I wrote an article about having trouble authenticating when trying to login to remote server with key authentication and steps to solves the problem.  The article is here:  
[SSH Authentication Refused Bad Ownership or Modes for Directory](https://www.bulafish.com/centos/2018/04/26/ssh-authentication-refused-bad-ownership-or-modes-for-directory/)

So I thought then I should write an article about how to ssh remote linux server with key authentication.

In this example, I prepare two demo server and the information is below

Name | IP
-|-
coffee  |  192.168.122.92
tea  |  192.168.122.187

![ssh-keygen](/assets/img/2018042705.png)
![ssh-keygen](/assets/img/2018042704.png)

The main idea of key authentication is

1. generate private/public key on the source server, in our case is `coffee`
2. copy the generated public key to destination server, which is `tea`

Before we start, let's take a look of the initial state first.  It is crucial to know the differences between before and after.  `ssh-keygen` will generate files to the logged in user home directory, so let's take a look of `coffee` and `tea`
<br>![ssh-keygen](/assets/img/2018042701.png)
![ssh-keygen](/assets/img/2018042707.png)

Now let's start generate the keys in `coffee` with command `ssh-keygen`.
```bash
ssh-keygen -t rsa -b 4096
```
![ssh-keygen](/assets/img/2018042702.png)

{% include ads3.html %}

From the image we learn that the generated files are saved at `/home/username/.ssh/` directory and the files are `id_rsa`, which is your private key, and `id_rsa.pub`, which is your public key.  ssh-keygen will prompt you three times, the first one is asking if you would like to specify a file to save the key rather the default value.  Usually we just go with the default value so just hit enter.

The second and third prompt is asking if you would like to set a password to your private key.  This is very important cause the private key is used to / can access to all servers that is setup with private key.  So if anyone get hold of your private key, that person has access to all your servers.  So it is very important to add passphrase to your private key for protection.  If you do not wish to add passphrase, just simpely hit enter at the prompts.

ssh-keygen can generate types of key such as

1. rsa
2. dsa
3. ecdsa
4. ed25519

For more information about the differences between each tyes, please refer here:  
[How to use ssh-keygen to generate a new SSH key | SSH.COM](https://www.ssh.com/ssh/keygen/)

Now let's check out `coffee's` home directory.  We can see that a new hidden directory `.ssh` is created with your `private` and `public keys` inside.
<br>![ssh-keygen](/assets/img/2018042703.png)

Next we need to copy the public key, which is `id_rsa.pub`, to `tea`.  Many articles would teach you to
1. Copy the content of the key.
2. Login to `tea`.
3. Create `.ssh` directory inside `user home directory` if it does not exists.
4. Create a file call `authorized_keys` inside .ssh directory.
5. Paste the copied content into authorized_keys.

Now all those troublesome steps can be simplified into just one command.
```bash
ssh-copy-id user@destination-server-ip-or-name
```
![ssh-copy-id](/assets/img/2018042708.png)

All you have to do is key in the password of user `tea` and ssh-copy-id will do the rest for you!

Now we are all set!  If everything is setup correctly, you should have the same result as the image shown below.
<br>![ssh-copy-id](/assets/img/2018042709.png)

REFERENCES:
<br>[How to use ssh-keygen to generate a new SSH key | SSH.COM](https://www.ssh.com/ssh/keygen/)
<br>[HowTos/Network/SecuringSSH - CentOS Wiki](https://wiki.centos.org/HowTos/Network/SecuringSSH)
<br>[ Generating a new SSH key and adding it to the ssh-agent - User Documentation](https://help.github.com/articles/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent/)
