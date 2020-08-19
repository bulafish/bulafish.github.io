---
title: Change Vim Color Scheme
author: bulafish
date: 2018-04-26 +0800
categories: [CentOS]
tags: [Configuration]
---

Let's take a look of the default vim color scheme first, using `/etc/vimrc` as example.  
![vim color scheme](/assets/img/2018050505.png)

We can see that a dim background with a dark color wording can make reading quite difficult sometimes, luckily there are seventeen vim color schemes that we can choose from to make our life easier.  Those config files are located at `/usr/share/vim/vim74/colors`.  
![vim color scheme](/assets/img/2018050502.png)

There are two location in the system where you can set/change the color scheme you want.  To change globally, meaning that the change will apply to all users in the system, `/etc/vimrc` is the file to modified.  For only certain user's color scheme, for example john, then `/home/john/.vimrc` is the file you modify.  For simplicity, I am just going to go globally.  
![vim color scheme](/assets/img/2018050504.png)

All you have to do is add `colorscheme choiceofcolor` or `colo choiceofcolor` at the bottom of the file.  For example, our first choice, blue.  
![vim color scheme](/assets/img/2018050503.png)

Then vim `/etc/vimrc` again to see the change of color and wow.. blue seem not like a good choice!  
![vim color scheme](/assets/img/2018050506.png)

So, that's it!  Now you just have to try another sixteen times, which is exactly what I did...  To save your time, I have all the result below so you can just look and pick your favorite one!  Use the picked color scheme to try on more files is recommended.

## darkblue
![vim color scheme](/assets/img/2018050507.png)

## default
![vim color scheme](/assets/img/2018050508.png)

## delek
![vim color scheme](/assets/img/2018050509.png)

## desert
![vim color scheme](/assets/img/2018050510.png)

## elflord
![vim color scheme](/assets/img/2018050511.png)

{% include ads3.html %}

## evening
![vim color scheme](/assets/img/2018050512.png)

## koehler
![vim color scheme](/assets/img/2018050513.png)

## morning
![vim color scheme](/assets/img/2018050514.png)

## murphy
![vim color scheme](/assets/img/2018050515.png)

## pablo
![vim color scheme](/assets/img/2018050516.png)

## peachpuff
![vim color scheme](/assets/img/2018050517.png)

## ron
![vim color scheme](/assets/img/2018050518.png)

## shine
![vim color scheme](/assets/img/2018050519.png)

## slate
![vim color scheme](/assets/img/2018050520.png)

## torte
![vim color scheme](/assets/img/2018050521.png)

## zellner
![vim color scheme](/assets/img/2018050522.png)

Alright!  That's preview for all the color schemes.  I guess I am going to go with `koehler` for now and if I can't get used to it, then will change to some other scheme.  So what's your choice?

REFERENCES:  
[How to set and use a vim color scheme](https://alvinalexander.com/linux/vi-vim-editor-color-scheme-colorscheme)
