---
title: Ncdu Linux Alternate Choice For Treesize
author: bulafish
date: 2018-05-03 +0800
categories: [CentOS]
tags: [Ops]
---

[Treesize](https://www.jam-software.com/treesize_personal/) is a very handy software that calculate all files and directories size and more other functions in windows system.  But since I am using linux system so I never get the chance to use it.  But I saw how my colleague use it and thought how sad treesize does not provide linux version.  So I google around and found a package call [ncdu](https://dev.yorhel.nl/ncdu), which provides the function that I was looking for!

Let's take a look of how ncdu look like after running it.  From image, ncdu is calculated from `/` directory and sizes of everything uder `/`.
![ncdu](/assets/img/2018050305.png)

To install ncdu and start it.
```bash
yum install ncdu -y
cd /
ncdu
```
![ncdu](/assets/img/2018050303.png)

{% include ads3.html %}

When ncdu is started, it will start to calculate the size of all files and directories `from your current location`.  For example, `/` or `/home` or `/var`.  
![ncdu](/assets/img/2018050304.png)

Right after calculation, you will see this image, which ncdu is start from `/`, with the final calculated information from your current location.  
![ncdu](/assets/img/2018050305.png)

Press `?` to see more options.  
![ncdu](/assets/img/2018050306.png)

Use arrow keys to navigate `up`, `down`, `left`(previous location), `right`(enter cursor location), and `enter` to enter cursor location.
![ncdu](/assets/img/2018050307.png)

Finally, press q to exit ncdu.  That's it, enjoying!

REFERENCES:  
[TreeSize Professional alternative for Linux : linuxquestions](https://www.reddit.com/r/linuxquestions/comments/3g3ywc/treesize_professional_alternative_for_linux/?st=jgqefvq5&sh=1f6c7f7b)
