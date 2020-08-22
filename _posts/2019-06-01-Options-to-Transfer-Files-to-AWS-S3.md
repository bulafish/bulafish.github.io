---
title: Options to Transfer Files to AWS S3
author: bulafish
date: 2019-05-28 +0800
categories: [AWS]
tags: [S3]
---


Couple days ago a request came in from sales department saying that a customer is seeking for an option to transfer their files to AWS S3 and asking me for some ideas.

Customer’s requirements are using FTP protocol and the expected HDD usage is about 10TB.

Prior to survey for solutions, the only way I know how to accomplish this request was to use EC2(Linux) with [s3fs](https://github.com/s3fs-fuse/s3fs-fuse){:target="_blank"} package. After some research, I came up with 4 more options which are:

1. [AWS SFTP](https://aws.amazon.com/sftp/?nc1=h_ls){:target="_blank"}, Refer: AWS Trasfer for SFTP Walk Through
2. [AWS Storage Gateway](https://aws.amazon.com/storagegateway/){:target="_blank"}, Refer: AWS Storage File Gateway Walk Through
3. [EC2 with AWS EFS](https://aws.amazon.com/efs/?nc1=h_ls){:target="_blank"}, Refer: AWS EFS Walk Through
4. [AWS DataSync](https://aws.amazon.com/datasync/){:target="_blank"}, Refer: AWS DataSync Basic Walk Through

For more information of each services, please visit the links provide above.

For the s3fs option, with the help of s3fs, I can mount S3 bucket as a mounting point on EC2(Linux), setup FTP service in EC2 and use S3 mounting point as the FTP file directory for data transferring.

But after setting it up and try some testings, I found that the result is not as satisfied as expected. I tried to copy an ISO file, which is a little over 4GB in size, from OS to S3. It success in the first try but failed for the rest with an error msg saying something about permission deny or privilege error. I did not dig into cause personally, I prefer native solutions more than 3rd party’s solution so I just go ahead and look into other options.

Rather than talk about how to setup/configure/deploy those options, I want to talk a little bit about pros and cons of each of them.

![AWS SFTP](/assets/img/QNhdEFm3l1LFxE1a2R2Lkg.png)

`AWS SFTP` is a [fully managed service](https://aws.amazon.com/managed-services/features/){:target="_blank"}, which means that AWS will take care of the infrastructure and maintain the service, users don’t have to worry a thing. Whereas for File Gateway, users need to deploy an `Gateway(EC2)` and maintain high availability and scalability if needed

![AWS EFS](/assets/img/68mjWQdgHmvMM-ILiGu7GQ.png)

Same thing goes to `EC2 with EFS`. Thought `EFS is also a fully managed service`, but users need to deploy an EC2 and integrate it with EFS where the H.A and scalability if needed, is also the user’s responsibility.

![AWS Storage Gateway](/assets/img/wbiYt4_FHCQ9B8tb9GtBXQ.png)

As for [AWS Storage File Gateway](https://docs.aws.amazon.com/storagegateway/latest/userguide/Requirements.html#requirements-host){:target="_blank"}, there are couple ways to deploy Gateway instance, which are

1. VMWare Esxi
2. Hyper-V
3. EC2

I saw some comments about using VMware native method to provide H.A for the instance but as I don’t have the environment to try it out so won’t say anything about it. I suppose Hyper-V should have some mechanism to provide H.A as well. In my case, I choose EC2, which in turn, would have the same H.A and scalability problem as other options.

![AWS DataSync](/assets/img/IKosVjgorgdRiksCpU-v2w.png)

Lastly, `AWS DataSync` is similar to Storage File Gateway. User needs to deploy an agent server in choice of `VMware ESXi` OR an `EC2`, then sync the data to S3 or EFS. This option would face the same H.A and scalability issue as well.

{% include ads3.html %}

`***`

Now let’s look at the cost of [SFTP](https://aws.amazon.com/sftp/pricing/?nc1=h_ls){:target="_blank"}, [Storage Gateway](https://aws.amazon.com/storagegateway/pricing/){:target="_blank"}, [EFS](https://aws.amazon.com/efs/pricing/){:target="_blank"} and [AWS DataSync](https://aws.amazon.com/datasync/pricing/){:target="_blank"}.

Calculating the cost of using certain AWS Service is always a huge job to me and I could never be sure if I got the figure correctly or not. So I always have to state it as a “`predicted value`” at the end of my work. So for this article, I am going list out the `obvious parts` of the cost of those services for comparision.

For `AWS SFTP` at Tokyo region, the cost break down as below
1. SFTP endpoints, US$0.3/hr
2. Data uploaded, US$0.04/GB
3. Data downloaded, US$0.04/GB

So, assume 10TB of data is upload and 5TB data is download in a month. The total cost for only using AWS SFTP, `NOT including` S3 Storage cost, would be

> 0.3 * 24 * 30 + 0.04 * 1024 * 10 + 0.04 * 1024 * 5 = 216 + 409.6 + 204.8 = **US$830.4/month**

For [Storage Gateway](https://docs.aws.amazon.com/storagegateway/latest/userguide/StorageGatewayConcepts.html){:target="_blank"}, there are actually three types of gateway services, which are `File`, `Tape` and `Volume Gateway`. Where Volume Gateway comes in two modes: `Cached` and `stored`. In my scenario, I will be talking about File Gateway and the cost of using it is break down as below

1. Data written to storage by Gateway, US$0.01/GB
2. Data transfer out, US$0.114/GB

Assuming 10TB of data is written to S3, which the cost of S3 is not included, 5TB of data is transfer out, the cost would be

> 10240 * 0.01 + 5120 * 0.114 = 102.4 + 583.68 = **US$686.08/Month**

As for `AWS EFS`, there are three types of usage cost, which are
1. Standard Storage, US$0.36/GB-Month
2. Infrequent Access Storage, US$0.054/GB-Month `PLUS` Infrequent Access Requests, US$0.012/GB transferred
3. Provisioned Throughput, US$7.2/MB/s-Month

For option 1, the cost is very straight forward, the total cost per month would be

> 10240 * 0.36 = **US$3686.4/Month**

Option 2, assuming 10TB of infrequent access storage with 5TB of infrequent access requests, the monthly cost would be

> 10240 * 0.054 + 5120 * 0.012 = 552.96 + 61.44 = **US$614.4**

For option 3, it is much more complicated, please use the [web page](https://calculator.aws/#/addService){:target="_blank"} provided by AWS to calculate the estimate cost. Be aware that the cost above doesn’t include EC2 cost as this option is worked with an EC2 instance.

Lastly, the cost of `AWS DataSync` is quite straight forward too. It is calculated by the data copied `TO` and `FROM` S3 and EFS, which is US$0.04/GB. So in my scenario, I will copy 10TB of data to S3 and assume that I will copy 10TB out as well, the total cost not including S3 would be

> 10240 * 2 * 0.04 = **US$819.2/Month**

In conclusion, though AWS SFTP has fully managed service advantages but the cost is 2nd highest if EFS standard storage option is taken into consideration. Whereas EFS infrequent storage and access has the cheapest cost. So in my opinion, there is no best choice here where H.A, scalability and low cost are co-exist at the same time.

I think I will go for some experiments to see if I can increase H.A and high scalability for services with lower cost!

If you have any/better idea/thoughts, please drop me a line and we can discuss about it :)