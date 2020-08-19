---
title: Change Command Line Prompt Color In CentOS7
author: bulafish
date: 2018-04-25 +0800
categories: [CentOS]
tags: [Configuraion]
---

While ago when I was watching someone's youtube and found that his terminal command line prompt is colored and I found it actually very useful cause sometimes when too many terminals are opened, it was actually very easy to get confused!  Therefore if colors could be added to command line prompt, it would be very helpful to distinguish between each terminal connections and somehow prevent you from entering commands to the wrong server.

Let's first take a look of the difference between before and after.

Before|After
-|-
![www.bulafish.com](/assets/img/2018042503.png)|![www.bulafish.com](/assets/img/2018042504.png)

Before we start, let's take a look at my hostname first.  I set it to a [FQDN](https://en.wikipedia.org/wiki/Fully_qualified_domain_name) purposely so we can see the differences later on.
<br>![hostnamectl status](/assets/img/2018042501.png)
<br>For more information about hostname usage, please visit :
<br>[Set Or Change Hostname In CentOS7](https://www.bulafish.com/centos/2018/04/24/set-or-change-hostname-in-centos7/)

Command line prompt setting is store in a variable called PS1, we can check the current(default) setting using
```bash
echo $PS1
```
![hostnamectl status](/assets/img/2018042502.png)

We can break the output into four parts which are :
1. `\u` ; prints out the current login username, which is root.
2. `\h` ; prints out the hostname, which is www, as shown in hostname status.  If we want to show the FQDN, we can change it to `\H`.
3. `\W` ; prints out the current working directory, which is ~, the home directory of login user.  If we want to show the full working directory, we can change it to `\w`.
4. `\$` ; prints `$` when user is login, `#` when root is login.

`$PS1` variable can be saved in `/etc/bashrc` which will affect all users or `/home/username/.bashrc` which will only affect that particular user.

Now we understand the basic components of the command line prompt, let's put up the seven components of the "after" result, which is `\u@\H:\w\$ `, and save it as `PS1='\u@\H:\w\$ '` at the bottom of `/etc/bashrc`.  Please note that there is a space after the `$` sign, so there will be a space between the `$` or `#` sign and the cursor.  The seven components are :
1. `\u`
2. `@`
3. `\H`
4. `:`
5. `\w`
6. `\$`
7. A space

{% include ads3.html %}

Re-login to check the result.
<br>![hostnamectl status](/assets/img/2018042505.png)

Now let's start adding colors to the prompt.  The basic syntax of a color is `\[\e[xx;yy;zzm\]` where :
1. `\[\e[m\]` is the structure of the color syntax.
2. `xx;yy;zz` represents the text formate, text color and text background color.  xx, yy and zz are unique numbers so you can put them in any order you wish.

A detailed color table is below :

Text Fromat|Text Color|Text Background Color
-|-|-
0: normal text|30: Black|40: Black
1: bold|31: Red|41: Red
4: Underlined text|32: Green|42: Green
|33: Yellow|43: Yellow
|34: Blue|44: Blue
|35: Purple|45: Purple
|36: Cyan|46: Cyan
|37: White|47: White

With all the infomatin we got,  we can break the "after" result into :
1. `\[\e[01;36m\]\u`
2. `\[\e[01;37m\]@`
3. `\[\e[01;33m\]\H`
4. `\[\e[01;37m\]:`
5. `\[\e[01;32m\]\w`
6. `\[\e[01;37m\]\$`
7. `\[\033[0;37m\] ` ( please note that there is a space after `]` sign )

So the final result will be as follow :
<br>`PS1='\[\e[01;36m\]\u\[\e[01;37m\]@\[\e[01;33m\]\H\[\e[01;37m\]:\[\e[01;32m\]\w\[\e[01;37m\]\$\[\033[0;37m\] '`

Put it at the bottom of `/etc/bashrc`, save and exit.  Re-login to see your terminal with pretty colorful prompt!!

REFERENCES:
<br>[BASH Shell: Change The Color of Shell Prompt on Linux or UNIX](https://www.cyberciti.biz/faq/bash-shell-change-the-color-of-my-shell-prompt-under-linux-or-unix/)
<br>[Linux: Bash Color Prompt](http://xahlee.info/linux/shell_color_prompt.html)
