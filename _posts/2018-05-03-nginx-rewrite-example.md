---
title: Nginx Rewrite Sample
author: bulafish
date: 2018-05-03 +0800
categories: [CentOS]
tags: [Nginx]
---

Requests were sent from RD department about providing multi languages function on our website.  But due to some "nature infrastructure code" issue, they want me to use urls to trigger corresponding language of the web page.

Since we use nginx, so I thought well, it should be quite simple and easy to achieve it with nginx rewrite function and regular expression, which I am not familiar with both at all.  And truth proved me wrong that it took me longer than I thought to fulfill the requirement.

It took me almost one and half working day to figure it out so I am writing this down for my own future reference and might help anyone in anyway, if.

There are two requirements
1. Append ?lang=xx at the end of the url when page login.html is requested, regardless of what url is entered.
2. Append ?lang=xx according to the sub domain entered when page login.html is requested.  For instance, if url is xx.example.com/login.html, rewrite to xx.example.com/login.html?lang=xx.

For requirement one, the nginx rewrite rule is as below.  Put the code in server configuration file, inside server block.
```nginx
if ($request_uri ~* "/login\.html$") {
    rewrite ^(.*/)login\.html$ $scheme://$host/login.html?en=true permanent;
}
```
![nginx rewrite](/assets/img/2018050301.png)

[$request_uri](https://www.webhosting24.com/understanding-nginx-request_uri/) = everything behind the domain name.  In my case, the $request_uri I am looking for is `/login.html`.  
[~\*](http://nginx.org/en/docs/http/ngx_http_rewrite_module.html#directives) = matching of a variable against a regular expression with case-insensitive matching.  
`/login\.html$` = the pattern I am looking to trigger the rewrite rule.  
`\` is the escaped character in regular express so that `.` is just a `.` rather then some special function.  
`$` means that the pattern must end with what is before it, which in my case is `/index.html`.

So basically, `if ($request_uri ~* "/login\.html$")` is saying if `index.html` is inside and at the end of the url requested by client, then rewrite.

Regarding rewrite,  
`^(.*)` means everything before login.html.  
`$scheme` = http or https.  
`$host` = [FQDN](https://en.wikipedia.org/wiki/Fully_qualified_domain_name).

{% include ads3.html %}

So basically `rewrite ^(.*/)login\.html$ $scheme://$host/login.html?en=true permanent;` is saying whatever url is entered ended with login.html, rewrite to the entered http protocal + :// + FQDN enter + /login.html?en=true.  

This do the trick.  so,  
http://hi.test.com/login.html rewrites to http://hi.test.com/login.html?lang=en  
https://yo.example.com/login.html rewrites to https://yo.example.com/login.html?lang=en  
http://hi.test.com/list.html does nothing.

For requirement two, the nginx rewrite rule is as below.  Put this code inside location / block.
```nginx
if ( $host = us.test.com){
  set $lang us;
}

if ( $host = jp.test.com){
  set $lang jp;
}

if ($args !~* lang){
  set $lang "${lang}t";
}

if ($lang = ust){
  rewrite /login.html /login.html?lang=us permanent;
}

if ($lang = jpt){
  rewrite /login.html /login.html?lang=jp permanent;
}      
```
![nginx rewrite](/assets/img/2018050302.png)

Since nginx `if` function does not support `and` and `or` statement, so I used variable to save the requested url.  
`$host` is the FQDN, save `jp` or `us` to variable `$lang` according to the FQDN entered.  

For some reason I would have infinite loop problem and I can't figure it out why, after some research, I figure out a way to break the loop, combined with a way to distinguish between jp and us.

`$args` = the variable appended to login.html.  In my case will be `lang`, which is used to store jp or us.  
`$args !~* lang` = if `lang` variable does not exist, then perform rewrite.  This is the method I found to break the loop.  
`set $lang "${lang}t"` = set $lang variable to the current value + t, so `$lang` would be either `ust` or `jpt`, use for later to rewrite the corresponding lang.  
Finally check if `$lang` is `ust` or `jpt`, then rewrite `/login.html` to `/login.html?lang=us` or `/login.html?lang=jp` accordingly!

I am sure there must be better and simplier and smarter way to achieve this but for now, this is the best solution I can figure out to fulfill my requirement.

REFERENCES:  
[Nginx how to multiple if statements](http://rosslawley.co.uk/archive/old/2010/01/04/nginx-how-to-multiple-if-statements/)  
[If Is Evil | NGINX](https://www.nginx.com/resources/wiki/start/topics/depth/ifisevil/)  
[Module ngx_http_rewrite_module](http://nginx.org/en/docs/http/ngx_http_rewrite_module.html#rewrite)  
[Making nginx check parameter in url](https://stackoverflow.com/questions/23988344/making-nginx-check-parameter-in-url?utm_medium=organic&utm_source=google_rich_qa&utm_campaign=google_rich_qa)
