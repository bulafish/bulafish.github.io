---
title: Install Goaccess Real Time Web Log Analyzer on CentOS7
author: bulafish
date: 2018-05-23 +0800
categories: [CentOS]
tags: [Installation]
---
What is [goaccess](https://goaccess.io/)?
>GoAccess is an open source real-time web log analyzer and interactive viewer that runs in a terminal in \*nix systems or through your browser.

Why goaccess?
>GoAccess was designed to be a fast, terminal-based log analyzer. Its core idea is to quickly analyze and view web server statistics in real time without needing to use your browser (great if you want to do a quick analysis of your access log via SSH, or if you simply love working in the terminal).

There are two ways to install goaccess, one from yum repository, one by downloading source file, compile and install.  The part I like goaccess the most is that it provides real time web log analysis in html form, so we can simply use browser to see the result in real time.

As mentioned before, there are two ways to install goaccess.  The differences between these two ways are if
1. your server runs in https
2. if you want geo location function

Then you go for the source code, compile and install it, otherwise, you can simply use yum to do so.  In this post, I am going to install it using source code.

Download source file from official website and decompress.
```bash
wget http://tar.goaccess.io/goaccess-1.2.tar.gz
tar -xzvf goaccess-1.2.tar.gz
cd goaccess-1.2/
```
![goaccess](/assets/images/2018052301.png)

Install necessary packages.
```bash
yum install gcc openssl-devel GeoIP-devel ncurses-devel
```
![goaccess](/assets/images/2018052302.png)

Compile goaccess with the function you want, available functions are below.
>--enable-debug  
Compile with debugging symbols and turn off compiler optimizations.  
--enable-utf8  
Compile with wide character support. Ncursesw is required.  
--enable-geoip=<legacy|mmdb>  
Compile with GeoLocation support. MaxMind's GeoIP is required. legacy will utilize the original GeoIP databases. mmdb will utilize the enhanced GeoIP2 databases.  
--enable-tcb=<memhash|btree>  
Compile with Tokyo Cabinet storage support. memhash will utilize Tokyo Cabinet's in-memory hash database. btree will utilize Tokyo Cabinet's on-disk B+ Tree database.  
--disable-zlib  
Disable zlib compression on B+ Tree database.  
--disable-bzip  
Disable bzip2 compression on B+ Tree database.  
--with-getline  
Dynamically expands line buffer in order to parse full line requests instead of using a fixed size buffer of 4096.  
--with-openssl  
Compile GoAccess with OpenSSL support for its WebSocket server.  

```bash
./configure --enable-utf8 --enable-geoip=legacy --with-openssl
```
![goaccess](/assets/images/2018052303.png)

If everything goes well, you will see the success built configuration.  
![goaccess](/assets/images/2018052304.png)

{% include ads3.html %}

Make and install goaccess.  Please use root privileges to run the following command.  I had trouble installing it even with sudo privileges.
```bash
make && make install
```
![goaccess](/assets/images/2018052305.png)

If everything goes fine, you will see the installed information.  
![goaccess](/assets/images/2018052306.png)

Goaccess is installed in the system now, let's do some checking.
```bash
which goaccess
goaccess -V
```
![goaccess](/assets/images/2018052307.png)

There are many ways to use goacces, please refer to [examples](https://goaccess.io/man#examples).  Next I will record the way I use it.  In order to save some typing, modify goaccess config file at `/usr/local/etc/goaccess.conf` as the image below, which will achieve
1. define the date, time and log format.  
![goaccess](/assets/images/2018052308.png)  
![goaccess](/assets/images/2018052309.png)
![goaccess](/assets/images/2018052310.png)
2. let goaccess run as daemon.  
![goaccess](/assets/images/2018052311.png)
3. output real time result in html form  
![goaccess](/assets/images/2018052312.png)
4. config ssl key-pairs and enable goaccess to run https  
![goaccess](/assets/images/2018052313.png)
![goaccess](/assets/images/2018052314.png)

You can set the `input` and `output` source as well at line 323 and 385 but I am going to leave these 2 blank.  Save changes and exit, use  command below to start goaccess, where the first part is your input file, with `-o` follow by output file, make sure output file is located at a place where it is internet accessible.
```bash
goaccess /var/log/nginx/demo.bulafish.com.access.log -o /usr/share/nginx/html/report/index.html
```
![goaccess](/assets/images/2018052315.png)

That's it, if everyting works right, you will see your real time statistics in html when you browse to your website.  
![goaccess](/assets/images/2018052321.png)

It is not a good idea to have your real time statistics data expose to internet so I am going to add an account password verification.  Use `htpasswd` to generate your account and password.  This command is provided by the `httpd-tools` package.
```bash
yum install httpd-tools
```
![goaccess](/assets/images/2018052316.png)

Now generate a file to where your web service has read privilege.  `-c` means use prompt to enter passwd, `.htpasswd` is the file to store account and password, it can be any name you like and the last is the account name.
```bash
htpasswd -c /usr/share/nginx/.htpasswd bulafish
```
![goaccess](/assets/images/2018052322.png)

Now we need to modify web service to adapt the verification setting.  In my case I use nginx, so I add a new location to `report` directory.
```nginx
location /report {
  auth_basic "Verification required to access";
  auth_basic_user_file /usr/share/nginx/.htpasswd;
}
```
![goaccess](/assets/images/2018052318.png)

Reload nginx and browse to your real time statistic web page, you will see a popup windows asking for account name and password and that it!!  
![goaccess](/assets/images/2018052319.png)
