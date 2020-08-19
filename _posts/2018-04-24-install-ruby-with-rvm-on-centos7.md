---
title: Install Ruby With Rvm On CentOS7
author: bulafish
date: 2018-04-24 +0800
categories: [CentOS]
tags: [Github pages]
---

Yesterday, we talked about how to get your [Github Pages](https://pages.github.com/) working, for more information, <br>please visit : [Use Github Pages To Build Your Static Website](https://www.bulafish.com/sharings/2018/04/23/use-github-page-to-build-your-static-website/).

Today, we are going to talk about how to prepare your environment ready for getting a clean jekyll theme which is used for Github Pages.  This tutorial is made base on CentOS system and we will be installing ruby with rvm.

Let's begin with installing rvm by adding trusted key.
```bash
gpg --keyserver hkp://keys.gnupg.net --recv-keys 409B6B1796C275462A1703113804BB82D39DC0E3 7D2BAF1CF37B13E2069D6956105BD0E739499BDB
```
![rvm installation](/assets/img/2018042304.png)

Next installing rvm.
```bash
curl -sSL https://get.rvm.io | bash -s stable
source /etc/profile.d/rvm.sh
```
![rvm installation](/assets/img/2018042306.png)

Now, rvm is done installing, let's start installing ruby.
```bash
rvm install ruby
```
![ruby installation](/assets/img/2018042307.png)

After installation, we can check the ruby status.
```bash
rvm list
ruby -v
```
![ruby installation](/assets/img/2018042308.png)

{% include ads3.html %}

Install bundler with gem for jekyll theme later usage.
```bash
gem install bundler
```
![ruby installation](/assets/img/2018042312.png)

Till now, we have done preparing the environment, let's install the very last package which is used to get the favorite theme of your choice!
```bash
yum install git -y
```
![ruby installation](/assets/img/2018042309.png)

Now, browse to the github page of the jekyll theme you chose and copy the project url.
<br>![jekyll theme installation](/assets/img/2018042310.png)

In case you have not chosen your theme yet, here are some locations where you can do so.
<br>[jekyllthemes.org](http://jekyllthemes.org/){:target="_blank"}
<br>[themes.jekyllrc.org](https://themes.jekyllrc.org/){:target="_blank"}
<br>[jekyllthemes.io](https://jekyllthemes.io/){:target="_blank"}

Next, clone the project to your terminal.  I chose a theme called next and cloned it into my /root/ directory.  Once downloaded, delete the Gemfile.lock file in the directory so there will be no error when we do bundle install next.
```bash
git clone https://github.com/Simpleyyt/jekyll-theme-next.git
cd jekyll-theme-next
rm -rf Gemfile.lock
bundle install
```
![jekyll theme installation](/assets/img/2018042313.png)

After bundle installation, we are all done!
Now we have a new and clean jekyll theme of our choice!
<br>Next I will show you all how to upload the theme we just made to GitHub Pages which we have created in last post, stay tuned!

RELATED POSTS:
<br>[Use Github Pages To Build Your Static Website](https://www.bulafish.com/sharings/2018/04/23/use-github-page-to-build-your-static-website/)
<br>[Upload Jekyll Theme To Github Pages](https://www.bulafish.com/sharings/centos/2018/04/24/upload-jekyll-theme-to-github-pages/)

REFERENCES:
<br>[RVM: Ruby Version Manager](https://rvm.io/)
