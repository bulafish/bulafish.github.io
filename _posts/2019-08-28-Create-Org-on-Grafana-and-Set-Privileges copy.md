---
title: Create Org on Grafana and Set Privileges
author: bulafish
date: 2019-08-28 +0800
categories: [Grafana7]
# tags: [S3]
---

## Goal：
Create Org, its user account and set user account privileges on Grafana.  All actions are done by Grafana admin.

## Steps：
1. Login Grafana as admin, create new Org
2. Create new users, set privileges, verify user and move to new Org
3. Add data source
4. Create / import and configure required dashboards for Org

## briefting：
Let's say you only have `A` Grafana server and you want to provide its services to many different entities, for example, different departments in your company or different customers that you provide services to, yet all of them can only see their own data.  Also, all of them are read accessible only.  Any kind of modification on data nor dashborad etc are prohibitted.

`***`

## Create Org
Login as Grafana admin, repeat the steps to create new Orgs, CustomerA and CustomerB
![Grafana](/assets/img/20200828001.png)

Name the Org<br>
![Grafana](/assets/img/20200828002.png)

![Grafana](/assets/img/20200828003.png)

Finish view of two new Orgs
![Grafana](/assets/img/20200828004.png)

## Create / configure new user
Create users belong to the org and set as viewer
![Grafana](/assets/img/20200828005.png)

Configure user information<br>
![Grafana](/assets/img/20200828006.png)

Finish view of two new users.  Click on user name to setup privileges and Org
![Grafana](/assets/img/20200828010.png)

Add user to the designated Org and assign read only privilege 
![Grafana](/assets/img/20200828011.png)

Remove user from main Org to prevent reading other metrics
![Grafana](/assets/img/20200828013.png)

{% include ads3.html %}

## Add data source
Switch to newly created Org<br>
![Grafana](/assets/img/20200828014.png)

Select Org
![Grafana](/assets/img/20200828015.png)

Add data source<br>
![Grafana](/assets/img/20200828016.png)

![Grafana](/assets/img/20200828018.png)

Select your data source, Prometheus in my case
![Grafana](/assets/img/20200828019.png)

Enter your Prometheus information, Save & test if Grafana is accessible to Prometheus
![Grafana](/assets/img/20200828020.png)

## Import / configure dashboard
Import dashboard with dashboard ID<br>
![Grafana](/assets/img/20200828021.png)

![Grafana](/assets/img/20200828023.png)

Enter your desired dashboard ID, click Load
![Grafana](/assets/img/20200828024.png)

Configure data source for the dashboard, click Import at last
![Grafana](/assets/img/20200828025.png)

At this moment, user in this Org is able to read everyone's metrics, so we need to restrict user in this Org only capable of reading its own data
![Grafana](/assets/img/20200828026.png)

## Restriction to read own data
Click setting on dashboard<br>
![Grafana](/assets/img/20200828027.png)

Click on variable "job"<br>
![Grafana](/assets/img/20200828028.png)

Input regex according to your needs.  A preview value is show at bottom
![Grafana](/assets/img/20200828029.png)

Save changes<br>
![Grafana](/assets/img/20200828031.png)
## Verify result
That's it!  Let's login user account to verify
![Grafana](/assets/img/20200828033.png)

We can see that configuration settings are gone and only own metrics, CustomerA, can be seen!