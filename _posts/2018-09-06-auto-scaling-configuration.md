---
title: Auto Scaling Configuration
author: bulafish
date: 2018-09-06 +0800
categories: [AWS]
tags: [Auto Scaling]
---

### Introduction
Steps to create auto scaling (A.S), two main concepts
1. Configuration
  * `What` to scale out
  * Instance settings related
2. Scaling groups
  * `Where` to scale out：VPC, subnet, LB etc
  * `How many` and `what condition` to scale：Scaling policies


### Create A.S Configuration
1. Select AMI
2. Select instance type
3. Configure details  
![auto scaling](/assets/images/2018090602.png)<br>
4. Select storage
5. Select security group (S.G)
6. Select pem
7. Completed view  
![auto scaling](/assets/images/2018090603.png)


### Create A.S Group
1. Select the source configuration for A.S group  
![auto scaling](/assets/images/2018090604.png)<br>
2. Configure details  
![auto scaling](/assets/images/2018090605.png)
![auto scaling](/assets/images/2018090606.png)<br>
3. Configure scaling policies, which can be set here or later  
![auto scaling](/assets/images/2018090607.png)<br>
4. Configure notification here or later    
![auto scaling](/assets/images/2018090608.png)<br>
5. Configure tags which will auto tag instance when scaling out  
![auto scaling](/assets/images/2018090609.png)


### Result
Right after setting up, A.S is creating an new instance due to the difference between desired and current instance quantity.  Click `Edit` to modify any setting accordingly  
![auto scaling](/assets/images/2018090610.png)

Clink `Instance` tag to view instances and its condition under this A.S  
![auto scaling](/assets/images/2018090611.png)

Confirm the instance status by checking the EC2 service main section  
![auto scaling](/assets/images/2018090612.png)

Check A.S `Activity History` to understand what A.S has done  
![auto scaling](/assets/images/2018090613.png)

{% include ads3.html %}


### Configure Scaling Policy
Click `Add policy` under` Scaling Policies` tag, click `Create simple scaling` policy  
![auto scaling](/assets/images/2018090614.png)

Give a name and set `take the action`, which is to add an instance.  
Condition, `Execute policy when`, can be set here but we will set it later elsewhere.  
![auto scaling](/assets/images/2018090615.png)

Repeat the step to create an `action` to decrease an instance.  
![auto scaling](/assets/images/2018090616.png)

Completed view of Scaling policies.  Notice that the `Execute policy when` has not been set yet, which we will do it elsewhere later.  
![auto scaling](/assets/images/2018090617.png)


### Create Alarm under CloudWatch (C.W)
Create alarms under C.W to monitor certain condition of instances and trigger action created on steps above when condition is met  
![auto scaling](/assets/images/2018090618.png)

In this example we will monitor the metrics in A.S group  
![auto scaling](/assets/images/2018090619.png)

Select the metric to monitor, in this case, CPUUtilization is selected  
![auto scaling](/assets/images/2018090620.png)

Set the condition to be triggered  
`Whenever` section is the place to set the condition  
`Actions` section is the place to set what to do when condition is met.  In this case, we want to scale out when condition is met, so delete the default action which is SNS notification and click on` AutoScaling Action`  
`Period` is the time range to be monitor  
![auto scaling](/assets/images/2018090621.png)

Completed view.  This configuration is saying
1. In every 1 minute monitored data point
2. If any monitored data point, wich is CPUUtilization, is more than 50%
3. Execute `Actions`, which is the scale policy action created in A.S group  
![auto scaling](/assets/images/2018090622.png)

Repeat the step to create a monitor policy to decrease instance when CPUUtilization is lower then 30%  
![auto scaling](/assets/images/2018090623.png)

Completed view of two alarms  
CPUUtilization is lower then 30% so lowCPU is in alarm state  
![auto scaling](/assets/images/2018090624.png)

Go back to `Scaling Policies` under A.S, the `Execute policy when` section has the condition information now which is the settings did in C.W  
![auto scaling](/assets/images/2018090625.png)


### A.S Testing
1. Increase instance CPU loading  
![auto scaling](/assets/images/2018090627.png)  
![auto scaling](/assets/images/2018090628.png)<br>
2. A.S responds to high CPU loading  
![auto scaling](/assets/images/2018090626.png)<br>
3. Checking C.W.  highCPU alarm is triggered so it executes the `scaling policy` set in A.S group  
![auto scaling](/assets/images/2018090629.png)<br>
4. Checking scaling policy in A.S.  
![auto scaling](/assets/images/2018090630.png)
