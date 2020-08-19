---
title: Upload Jekyll Theme To Github Pages
author: bulafish
date: 2018-04-24 +0800
categories: [Others]
tags: [Github pages]
---

Finally, we are at the final stage of making github pages to work with our own choice of jekyll theme.  In this tutorial, we are going to
1. Clone your github pages repository to your local computer.
2. Copy all the files created in [tutorial 2](https://www.bulafish.com/sharings/centos/2018/04/24/install-ruby-with-rvm-on-centos7/) into your local repository directory.
3. Push all the files from your local repository directory to github pages repository.
4. Enjoy!

## Clone your github pages repository
Use the command below to clone your repository, remember to replace `username` to your github account.  In the image below, you would see that I have the theme cloned along with my github pages repository.
```bash
git clone https://github.com/username/username.github.io
```
![git clone](/assets/img/2018042320.png)

## Copy jekyll theme files to your repository
Please replace `username` to your github account as well.
```bash
rsync -a jekyll-theme-next/* username.github.io/
cd username.github.io/
```
## Push files to github pages
Before push, we need to configure git command first, just simply enter your email and git account username as below.
```bash
git config --global user.email "your@email.com"
git config --global user.name "username"
```
![git config](/assets/img/2018042322.png)

{% include ads3.html %}

Next we add all the files ready to be pushed, please make sure you are inside your local github pages repository folder, follow by giving it a comment.
```bash
git add --all
git commit -m "put the comments you want here"
```
![git](/assets/img/2018042323.png)

And finally we can push it to github pages.  Authentication is required, just enter your github account and password.
```bash
git push origin master
```
![git push](/assets/img/2018042324.png)

## Enjoy!
Lastly, enter your username.github.io in the browser and you will see your brand new static website!
<br>![github pages](/assets/img/2018042325.png)

{% include ads1.html %}

RELATED POSTS:
<br>[Use Github Pages To Build Your Static Website](https://www.bulafish.com/sharings/2018/04/23/use-github-page-to-build-your-static-website/)
<br>[Install Ruby With Rvm On CentOS7](https://www.bulafish.com/sharings/centos/2018/04/24/install-ruby-with-rvm-on-centos7/)
