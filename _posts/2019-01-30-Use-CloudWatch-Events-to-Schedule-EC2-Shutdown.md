---
title: Use CloudWatch Events to schedule EC2 Shutdown
author: bulafish
date: 2019-01-30 +0800
categories: [AWS]
tags: [CloudWatch]
---

Introduction:<br>
CloudWatch Events is an event driven service that allows you to
1. Monitoring particular services with event types and to trigger actions to chosen target
2. Use crontab like function to schedule desired actions to chosen target

Objective:<br>
Use the `Schedule` function to power off instance every weekday at 1900 CST

Steps:<br>
1. Get the instance id that will be power off
2. Configure CloudWatch Events
* Choose Schedule function
* Choose and set crontab expression
* Configure function / action to perform and the target

![CloudWatch Events](/assets/images/Xnip2019-01-30_15-34-36.png)

Navigate to CloudWatch Events Rules
![CloudWatch Events](/assets/images/Xnip2019-01-30_15-15-52.jpg)

`Event Pattern`: For particular service and event function<br>
`Schedule`: for actions to perform regularly.  Choose this option for this post.

For Schedule, we can use a fixed rate of Minutes, Hours or Days.  If you need more complicated conditions, you can use the `Cron expression`.  The Cron expression is pretty much the same as regular linux crontab except that it has the `6th value` which represent `Year`.  Refer [Cron Expressions](https://docs.aws.amazon.com/AmazonCloudWatch/latest/events/ScheduledEvents.html#CronExpressions) for more details.
![CloudWatch Events](/assets/images/Xnip2019-01-30_15-44-07.png)

{% include ads3.html %}

For our objective, we will use Cron expression and set the expression as `00 11 ? * 2-6 *`
1. The configure time is on UTC, so I must +8 so it will reflect to CST
2. Date-of-month uses `?`, AWS doc says that it means one or another, but I take it as "doesn't matter which date"
3. Month uses * which means all
4. Day-of-week use 2-6 means Monday to Friday.  Sat is 7, Sun is 1
5. Year use * which means all

Once the value is set, a list will show below for you to verify the result, you can adjust the expression to correct it.
![CloudWatch Events](/assets/images/Xnip2019-01-30_16-31-39.png)

The last step is to configure the target and action to perform at scheduled time.
1. From the drop down list, there are a lot of functions / services can be chosen from.  Here we choose `EC2 StopInstances API call`, which fits our objective.
2. Input the target instance id for the action to perform to
3. In order for CloudWatch Events to perform action (StopInstances) on EC2, proper role needs to be given.  Here we can choose to create a new one or an existing role that has appropriate privileges for the action.

![CloudWatch Events](/assets/images/Xnip2019-01-30_16-37-15.png)

Give the rule a name and description and that's it!
![CloudWatch Events](/assets/images/Xnip2019-01-30_16-45-30.png)

A finishing view
![CloudWatch Events](/assets/images/Xnip2019-01-30_16-46-02.png)

REFERENCES:
<br>[What is Amazon CloudWatch Events](https://docs.aws.amazon.com/AmazonCloudWatch/latest/events/WhatIsCloudWatchEvents.html)
