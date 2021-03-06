---
title: Notify When Someone Login From Console
author: bulafish
date: 2019-03-29 +0800
categories: [AWS]
tags: [CloudTrail]
---

## Introduction:
CloudTrail is a service that records all AWS API activities, hence we can use this service combined with other services which are S3, CloudWatch Logs and SNS to accomplish a function that will notify you when someone login from web console

## Objective:
Be notified when someone login from console

## Steps:
1. Create SNS with email option to receive notification when someone login from web console
2. Create CloudTrail trail
  * Configure a S3 bucket to save the trail log
3. Configure trail to send logs to CloudWatch Logs
4. Create filter for CloudWatch Logs
5. Create CloudWatch Alarm for metric

## Create SNS with email option
Navigate to SNS
![SNS](/assets/img/Xnip2019-03-30_15-13-40.png)

Click Topics and create new topic
![SNS](/assets/img/Xnip2019-03-30_15-14-40.png)<br>
![SNS](/assets/img/Xnip2019-03-30_15-15-45.png)

Give a name to topic and leave the rest options default
![SNS](/assets/img/Xnip2019-03-30_15-17-13.png)<br>
![SNS](/assets/img/Xnip2019-03-30_15-17-32.png)

Create a subscription for the topic just created
![SNS](/assets/img/Xnip2019-03-30_15-19-06.png)

Choose email for Protocol option, we need to confirm the subscription from the email entered
![SNS](/assets/img/Xnip2019-03-30_15-19-49.png)<br>
![SNS](/assets/img/Xnip2019-03-30_15-22-38.png)

Finish view, the status is pending cause we need to confirm the subscription
![SNS](/assets/img/Xnip2019-03-30_15-25-14.png)

Login your email box and to confirm subscription
![SNS](/assets/img/Xnip2019-03-30_15-26-40.png)

If you see this page, it means subscription is successful and you will the status change to Confirmed
![SNS](/assets/img/Xnip2019-03-30_15-27-31.png)<br>
![SNS](/assets/img/Xnip2019-03-30_15-28-26.png)

## Create CloudTrail trail
Navigate to CloudTrail service
![CloudTrail](/assets/img/Xnip2019-03-30_12-08-47.png)

Create a trail
![CloudTrail](/assets/img/Xnip2019-03-30_12-10-03.png)

Give trail a name and configure options to fit needs
![CloudTrail](/assets/img/Xnip2019-03-30_12-12-16.png)<br>
![CloudTrail](/assets/img/Xnip2019-03-30_12-12-33.png)

Create a new or choose an existing S3 bucket to save trail log
![CloudTrail](/assets/img/Xnip2019-03-30_12-15-24.png)

{% include ads3.html %}

## Configure trail to send logs to CloudWatch Logs
On the trail list page, click the trail just created
![CloudTrail](/assets/img/Xnip2019-03-30_12-17-21.png)

Scroll all the way to bottom to configure CloudWatch Logs
![CloudTrail](/assets/img/Xnip2019-03-30_12-17-50.png)

Give CloudWatch Logs a name
![CloudTrail](/assets/img/Xnip2019-03-30_12-32-01.png)

Since CloudTrail trail will send logs to CloudWatch Logs, so we need to assign a role with appropriate privileges to CloudTrail trail
![CloudTrail](/assets/img/Xnip2019-03-30_12-33-34.png)

Finish view
![CloudTrail](/assets/img/Xnip2019-03-30_12-34-55.png)

## Create filter for CloudWatch Logs
Navigate to CloudWatch
![CloudWatch Logs](/assets/img/Xnip2019-03-30_12-35-28.png)

Click Logs then filters of the Logs we just created
![CloudWatch Logs](/assets/img/Xnip2019-03-30_12-37-54.png)

Add a metric filter
![CloudWatch Logs](/assets/img/Xnip2019-03-30_12-38-10.png)

On the define page, we make a test first.  evenName is what we will be filtering, so we pick an evenName from sample Log Data and test if we can correctly filter out results
![CloudWatch Logs](/assets/img/Xnip2019-03-30_12-42-36.png)

After we succeed, change evenName to ConsoleLogin
![CloudWatch Logs](/assets/img/Xnip2019-03-30_12-43-51.png)

Give a name to the filter and finish with some configuration
![CloudWatch Logs](/assets/img/Xnip2019-03-30_12-45-10.png)

Finish view
![CloudWatch Logs](/assets/img/Xnip2019-03-30_12-45-27.png)

For testing purpose, on the Log list page, set retention day to the logs
![CloudWatch Logs](/assets/img/Xnip2019-03-30_12-47-50.png)<br>
![CloudWatch Logs](/assets/img/Xnip2019-03-30_12-48-07.png)<br>
![CloudWatch Logs](/assets/img/Xnip2019-03-30_12-48-25.png)

Finish view
![CloudWatch Logs](/assets/img/Xnip2019-03-30_12-48-54.png)

Till now, we have completed the process of when someone login through console, API activity will be trailed, saved to S3 and push to CloudWatch Logs.  Now, re-login through console and wait for about 15 mins to see if everything works correctly.  If so, you will see the metric in CloudWatch Metric

## Create CloudWatch Alarm for metric
Create alarm from CloudWatch home page
![CloudWatch Alarms](/assets/img/Xnip2019-03-30_13-52-06.png)

Select the metric to monitor
![CloudWatch Alarms](/assets/img/Xnip2019-03-30_13-52-52.png)

Select the metric that is generated by filtering logs from trail
![CloudWatch Alarms](/assets/img/Xnip2019-03-30_13-53-22.png)<br>
![CloudWatch Alarms](/assets/img/Xnip2019-03-30_13-53-34.png)<br>
![CloudWatch Alarms](/assets/img/Xnip2019-03-30_13-55-05.png)

Change metric settings
![CloudWatch Alarms](/assets/img/Xnip2019-03-30_15-10-38.png)<br>
![CloudWatch Alarms](/assets/img/Xnip2019-03-30_15-12-06.png)

Give a name and set the threshold of this alarm
![CloudWatch Alarms](/assets/img/Xnip2019-03-30_15-30-48.png)

Set action to trigger when threshold is reached, which is the SNS we created in the first place
![CloudWatch Alarms](/assets/img/Xnip2019-03-30_15-32-47.png)

Finish view
![CloudWatch Alarms](/assets/img/Xnip2019-03-30_15-33-50.png)

Lastly, re-login through console again and after around 15 mins, you will receive notification through mail
![CloudWatch Alarms](/assets/img/Xnip2019-03-30_16-02-09.png)

REFERENCES:
<br>[AWS: How to get notified on IAM user logins](https://advancedweb.hu/2019/02/05/iam_login_notification/)
<br>[How to Receive Notifications When Your AWS Account’s Root Access Keys Are Used](https://aws.amazon.com/blogs/security/how-to-receive-notifications-when-your-aws-accounts-root-access-keys-are-used/)
