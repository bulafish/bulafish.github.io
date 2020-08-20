---
title: Use CloudWatch to Monitor Billing
author: bulafish
date: 2019-02-06 +0800
categories: [AWS]
tags: [CloudWatch]
---

Introduction:<br>
CloudWatch is a very handy monitoring system and it can monitor a lot of metrics in the AWS system.  This post is going to show you how to monitor your billing and send out notification if the billing excesses the threshold you set

Objective:<br>
Set a threshold to your billing and use CloudWatch to monitor it and once the billing excesses the threshold, send out SNS for notification

Steps:<br>
1. Set to receive billing alert
2. Configure CloudWatch alert
* Set threshold
* Set SNS
* Confirm SNS email notification

First, from upper right hand corner, click your account name and choose `My Billing Dashboard`
![CloudWatch billing](/assets/img/Xnip2019-02-06_20-11-04.png)

To be able to receive billing excesses notification, we must first enable to receive it.  From Billing Dashboard, go to `Preferences`, check the option of `Receive Billing Alerts`
![CloudWatch billing](/assets/img/Xnip2019-02-06_20-12-05.png)

Now we go to CloudWatch to configure out conditions
![CloudWatch billing](/assets/img/Xnip2019-02-06_20-13-44.png)

From CloudWatch, go to `Billing`
![CloudWatch billing](/assets/img/Xnip2019-02-06_20-14-14.png)

{% include ads3.html %}

Pay attention that all billing data and alarms are displaying from N.Virginia region only, so console will prompt you to switch region if you are in a different region
![CloudWatch billing](/assets/img/Xnip2019-02-06_20-14-47.png)

Once you are at N.Virginia, you can create an alarm.  Pay attention that there are only 10 free alarms and 1000 free e-mails for each month
![CloudWatch billing](/assets/img/Xnip2019-02-06_20-16-56.png)

When creating an alarm, you need to set the threshold and which email to notify if the threshold is excessed.  In this example, I set the threshold at US$1 and you can choose the existing SNS topic if you already have one
![CloudWatch billing](/assets/img/Xnip2019-02-06_20-27-03.png)

Or if you don't have any existing SNS topic or would like to set a new email for notification, you can use the `New list` and enter the desired email
![CloudWatch billing](/assets/img/Xnip2019-02-06_20-28-17.png)

Once the alarm is created, a confirmation email is sent to the appointed email
![CloudWatch billing](/assets/img/Xnip2019-02-06_20-29-08.png)

Login to the configured email account to confirm the subscription, it looks like below
![CloudWatch billing](/assets/img/Xnip2019-02-06_20-30-30.png)

Once the email is confirmed, the alarm dialog windows will be update as below
![CloudWatch billing](/assets/img/Xnip2019-02-06_20-31-40.png)

That's it! the billing monitoring is Completed
![CloudWatch billing](/assets/img/Xnip2019-02-06_20-34-58.png)
