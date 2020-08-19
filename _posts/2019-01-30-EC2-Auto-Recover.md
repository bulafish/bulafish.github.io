---
title: EC2 Auto Recover
author: bulafish
date: 2019-01-30 +0800
categories: [AWS]
tags: [EC2]
---

Introduction:<br>
You can create a `CloudWatch alarm` that monitors an Amazon EC2 instance and automatically recovers the instance if it becomes `impaired` due to an underlying hardware failure or a problem that requires AWS involvement to repair. Terminated instances cannot be recovered. A `recovered instance is identical` to the original instance, including the instance ID, private IP addresses, Elastic IP addresses, and all instance metadata. If the impaired instance is in a placement group, the recovered instance runs in the placement group.

From EC2 console, select `Monitoring` tag, then select `Create Alarm`
![auto recover](/assets/images/Xnip2019-01-30_13-05-25.jpg)

When creating alarm, there are quit a few of action that we can actually choose from, as shown in the image, but we are going to do auto recover only.  When creating alarm, we configure
1. Send notification if action is triggered (`Optional`)
2. What action to perform when monitored condition is met
3. The monitoring metric and threshold
4. Name of the alarm

![auto recover](/assets/images/Xnip2019-01-30_13-14-24.jpg)

{% include ads3.html %}

Pay attention that for auto recover, CloudWatch alarm monitors the status of `System Status Checks` instead of `Instance Status Checks`.  AWS uses these two metrics to check and identify the health of the instance.
<br>`System Status Checks`: Check the connectivity of the instance.  If it fails, it means that part of the underlaying infrastructure is failed.
<br>`Instance Status Checks`: Check if the OS is accepting traffic.  It if fails, it means something is wrong with the OS level.
![auto recover](/assets/images/Xnip2019-01-30_13-44-31.jpg)

A finishing view, click the alarm's name to redirect to CloudWatch Alarms
![auto recover](/assets/images/Xnip2019-01-30_13-14-55.jpg)

Once at CloudWatch Alarms page, select the alarm to get more detailed information.  Alarm can be modified by using `Modify` inside `Actions` tag on the top of the windows.
![auto recover](/assets/images/Xnip2019-01-30_13-27-31.jpg)

Once the alarm is configured, it can be seen under the `Monitoring` tag inside EC2 Instances page too.
![auto recover](/assets/images/Xnip2019-01-30_13-05-25.jpg)

And that's it.  CloudWatch will keep monitoring the `System Status Checks` metric and if, in this post, the check fails for two minutes continuously, CloudWatch alarm will trigger the `Recover this instance` action and migrate(should be stop then start) the instance to different host.

REFERENCES:
<br>[Recover Your Instance](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-instance-recover.html)
