---
title: Use Line Notify to Notify When Someone Login Server
author: bulafish
date: 2018-07-17 +0800
categories: [CentOS]
tags: [Ops]
---

## Goal：
Use line notify to notify you when someone login your servers.

## Steps：
1. Generate Line Token
2. Config PAM
3. Notifying Code

## Generate Line Token：
Create a line group first, line notify only work for line group.  
Login https://notify-bot.line.me with your line account.

Click your name on upper right hand corner, then click `My page`.  
![line notify](/assets/img/2018071701.png)

Scroll to the bottom and click `Generate token`.  
![line notify](/assets/img/2018071702.png)

Enter the name showing to notify you.  Check the last images if you ain't sure what to key.  
Pick the line group to be notified  .
Click `Generate token`.  
![line notify](/assets/img/2018071703.png)

Your line token will be prompted, copy it down and don't lose it, it is not recoverable.  
Your line notify service will be created and shown on the list.  
![line notify](/assets/img/2018071704.png)

## Config PAM：
As our goal is to notify you when someone login your server, so that means [PAM](https://en.wikipedia.org/wiki/Linux_PAM) is involved.  Under `/etc/pam.d/` are all the services that is using PAM, append the following line to `/etc/pam.d/sshd` file.
```bahs
session optional pam_exec.so type=open_session seteuid /usr/bin/python /home/login.py
```
The brief meaning of above line is if someone is login (open_session), then run (pam_exec) /usr/bin/python /home/login.ph.  For more information, please refer [pam_exec](http://www.linux-pam.org/Linux-PAM-html/sag-pam_exec.html).

{% include ads3.html %}

## Notifying Code：
Here I use python, the code is below, python-requests package is required.

```python
import requests, os

def lineNotify(token, msg):
    url = "https://notify-api.line.me/api/notify"
    headers = {
        "Authorization": "Bearer " + token
    }

    payload = {'message': msg}
    r = requests.post(url, headers = headers, params = payload)
    return r.status_code

# replace with your token
token = "xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx"

# modify the output message according to your needs
msg = os.environ.get('PAM_USER')+" "+os.environ.get('PAM_SERVICE')+"\n"+os.uname()[1]+"\nfrom "+os.environ.get('PAM_RHOST')

lineNotify(token, msg)
```

I printed out `os.environ` and got the following variables.
1. XDG_RUNTIME_DIR
2. PAM_USER
3. PAM_SERVICE
4. SELINUX_LEVEL_REQUESTED
5. PAM_RHOST
6. PAM_TTY
7. SELINUX_ROLE_REQUESTED
8. SELINUX_USE_CURRENT_RANGE
9. PAM_TYPE
10. XDG_SESSION_ID

I want the message to look like, `who is using what service to login which server from where`, so using the variables above to compose the `msg` variable in the code.

## Outcome
![line notify](/assets/img/2018071705.png)  
`notify` is the name you key in when generating the line token, so it would be helpful if an appropriate name is used.


REFERENCES:  
[透過 Python 發 Line Notify 三部曲 - 發圖圖圖片](http://pythonorz.blogspot.com/2017/12/python-line-notify_18.html)
