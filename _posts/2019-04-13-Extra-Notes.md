---
title: AWS Self Learning Part6
author: bulafish
date: 2019-04-13 +0800
categories: [Study Note]
# tags: []
---

# Cognito
* Use identity pools for <mark>authenticated</mark> and <mark>unauthenticated (guest)</mark> user
* User directory service for user-sign or sign-up for mobile or application

# SNS
* Producers and Consumers communicate <mark>asynchronously</mark> with subscribers by producing and sending a message to a topic
* Producers push messages to the topic, they created or have access to, and SNS matches the topic to a list of subscribers who have subscribed to that topic, and delivers the message to each of those subscribers
* SQS Queues
	* SNS with SQS provides the ability for messages to be delivered to applications that require immediate notification of an event, and also persist in an SQS queue for other applications to process at a later time
	* SNS allows applications to send time-critical messages to multiple subscribers through a “push” mechanism, eliminating the need to periodically check or “poll” for updates.
	* SQS can be used by distributed applications to exchange messages through a polling model, and can be used to decouple sending and receiving components, without requiring each component to be concurrently available 

# SQS
* SQS queue retains messages for four days, by default
* Queues can configured to retain messages for 1 minute to 14 days after the message has been sent
* Receipt handle is required, not the message id, to delete a message or to change the message visibility
* If a message is received more than once, each time its received, a different receipt handle is assigned and the latest should be used always
* SQS does not delete the message once it is received by a consumer, Consumer should explicitly delete the message from the Queue once it is received and successfully processed
* default Visibility timeout for each Queue is 30 seconds and can be changed at the Queue level, max is 12 hours
* SQS can delete a queue without notification if one of the following actions hasn’t been performed on it for 30 consecutive days.
* SQS allows the deletion of the queue with messages in it
* Dead Letter Queues
	* Dead letter queue is a queue for messages that were not able to be processed after a maximum number of attempts
* Locks the message during processing, using Visibility Timeout, preventing it to be processed by any other consumer
* Standard queue
	* Delivery at least once
	* nearly-unlimited number of transactions per second
	* SQS Buffered Asynchronous Client ; <mark>Not Support FIFO</mark>
* FIFO queue
	* limited to 300 transactions per second per API action
	* Guarantee exactly delivery once
	* FIFO delivery
	* FIFO Queue name should end with .fifo
	* FIFO queues support message groups that allow multiple ordered message groups within a single queue
	* SQS Buffered Asynchronous Client doesn’t currently support FIFO queues
	* Not all the AWS Services support FIFO
	* Duplicates introduced by the message producer are removed within a 5-minute deduplication interval
	* Exactly-once processing
		* A message is delivered once and <mark>remains available until a consumer processes and deletes it</mark>
		* Key when duplicates can’t be tolerated.
		* Limited to 300 transactions per second (TPS) without batching
		* With batching, 3000 messages per second
	* Message group
		* For each message group ID, all messages are sent & received in strict order
		* However, messages with different message group ID values might be sent and received out of order
	* Dead Letter Queue is designed to store messages that can't be processed
	* If DLQ retention day is set to 4, the message is in normal queue then send to DLQ, it will be deleted after 2 days cause normal 2 days + DLQ 2 days = total retention 4 days

# AWS Directory Service
* Managed Microsoft AD
	* Actual Microsoft Active Directory Service in AWS cloud
	* LDAP support for linux application
	* Application or AWS service such as WorkSpace, WorkDoc, Chime, Connect, QuickSight and SQL for RDS
	* Enable you to sign in AWS console
	* SSO provides short-term credential for SDK or CLI and preconfigured SAML integrations to sign in to many cloud applications
	* By adding Azure AD Connect, and optionally Active Directory Federation Service (AD FS), you can sign in to Microsoft Office 365 and other cloud applications with credentials stored in AWS Managed Microsoft AD
	* Provide MFA capability for Managed AD
	* Enable you to extend schema, manage password policies, and enable secure LDAP communications through SSL and TSL
* AD Connector
	* best choice when want to use your existing on-premises directory with compatible AWS services
	* A proxy service provides easy way to connect compatible AWS applications, such as WorkSpaces, QuickSight, and EC2 for Windows, to your existing on-premises Microsoft AD
	* If only need on-premise user to log in AWS application and services with their AD credential
	* Can be used to join EC2 to existing AD domain
	* When sign in to AWS SaaS, sign in request is sent to on-premise AD for authentication
	* Can join windows EC2 to on-premise domain by seamless domain join
	* Allow user to access console and manage resources by logging with existing AD credential
	* Provide MFA by using your existing RADIUS-base MFA infrastructure
	* With AD Connector, you continue to manage your Active Directory as you do now
	* NOT compatible with RDS SQL Server
* Simple AD
	* Standalone directory in cloud
	* Can sign in to Console with Simple AD credential to manage AWS resources
	* Low scale, low cost with basic AD compatibility that supports Samba 4 compatible application OR LDAP compatible
	* NOT compatible with RDS SQL Server
	* AWS provides monitoring, daily snap-shots, and recovery as part of the service
	* DOES NOTsupport
		* MFA
		* Trust relationships
		* DNS dynamic update
		* Schema extensions
		* Communication over LDAPS
		* PowerShell AD cmdlets
		* FSMO role transfer
* Amazon Cloud Directory
	* If need cloud-scale directory to share and control access to hierarchical data between your applications
	* Can store hundreds of millions of application-specific objects with multiple relationships and schemas
* Cognito
	* If develop SaaS application and need directory to manage and authenticate your user and that works with <mark>social media identities</mark> such as FB, Google and Amazon

# Redshift
* Redshitf has following logs
	* Connection log
		* logs authentication attempts, and connections and disconnections
	* User log
		* logs information about changes to database user definitions
	* User active log
		* logs each query before it is run on the database
* Support auto backup


# KMS
* Before deleting CMK, you need to
	* Examining CMK Permissions to Determine the Scope of Potential Usage
	* Examining AWS CloudTrail Logs to Determine Actual Usage
* For CMK, auto key rotation period is only every year
* When creating CMK, you can choose who can mange this key and who can use this key
* Key policy for CMK :
	* Key administrators : Who can administrate this key
	* Key users : Who can use this key
	* Key deletion : whether key administrator can delete this key
	* Other AWS accounts : allowing other AWS <mark>root</mark> account to use this key
	* CMK can only be deleted by scheduling with min of 7 days to max of 30 days
	* Deletion can be cancelled within scheduling day
* KMS Keys are only stored and used in the region in which they are created, can't transfer to other region
* KMS is integrated with CloudTrail, so all requests to use the keys are logged to understand who used which key when
* Rotation of keys
	* Keys generate by KMS rotated by KMS automatically
	* Previos verion of keys are kept for decryption of previous data
	* New data are encryoted and decrypted with new key
	* If manually rotated, data <mark>has to be re-encrypted</mark> depending on the application configuration
	* Automatic key rotation is not support for imported keys
* Data is submitted to AWS KMS to be encrypted, or decrypted, under keys that you control
* Envelope encryption is an optimized method for encrypting data that uses <mark>two different keys</mark>
* Envelop encryption
	* Data key is generated and used to encrypted data
	* Data key is then encrypted by master key defined in KMS
	* Encrypted data key is stored with <mark>AWS services</mark>
	* For data decryption, encrypted data key is sent to KMS to be decrypted by the master key, then AWS service can used decrypted data key to decrypt encrypted data
* When the data is encrypted directly with KMS it must be transferred over the network.
* Envelope encryption reduces the network load for the application or AWS cloud service as Only the request and fulfillment of the data key through KMS must go over the network
* KMS features
	* Create keys with a unique alias and description
	* Import your own keys
	* Control which IAM users and roles can manage keys
	* Control which IAM users and roles can use keys to encrypt & decrypt data
	* Choose to have AWS KMS automatically rotate keys on an annual basis
	* Temporarily disable keys so they cannot be used by anyone
	* Re-enable disabled keys
	* Delete keys that you no longer use
	* Audit use of keys by inspecting logs in AWS CloudTrail

# Route53
* Simple routing policy – Use for a single resource that performs a given function for your domain, for example, a web server that serves content for the example.com website.
* Failover routing policy – Use when you want to configure active-passive failover.
* Geolocation routing policy – Use when you want to route traffic based on the location of your users.
* Geoproximity routing policy – Use when you want to route traffic based on the location of your resources and, optionally, shift traffic from resources in one location to resources in another.
	* To use geoproximity routing, you must use Route 53 <mark>traffic flow</mark>
* Latency routing policy – Use when you have resources in multiple AWS Regions and you want to route traffic to the region that provides the best latency.
* Multivalue answer routing policy – Use when you want Route 53 to respond to DNS queries with up to eight healthy records selected at random.
* Weighted routing policy – Use to route traffic to multiple resources in proportions that you specify ; including failover function

# EBS
* CloudWatch Events
	* Create, delete, attach or reattach
	* Check if event is shown under Cloudwatch Events
* Encryption
	* happens on EC2, provide encrytion of data in transit between EC2 and EBS
	* Support all types of volume <mark>BUT NOT</mark> all types of EC2
	* CMK must be shared for encrypted snapshots.  Others must make copy of your entrypted snapshot, then make volume from it
* Volume
	* AZ specified, to move volume, make snapshot
	* Replicated <mark>WITHIN</mark> AZ across data centers
	* Decreasing volume size is not support
* Snapshot
	* Can be copied to regions, only can change from unencrypted to encrypted
	* Can create volume to AZs
	* To create AMI
	* Can be shared with multi AWS accounts or to public ( encrypted can't )
	* Snapshot to S3, only used data space is charged.  Only modified data is snapshot to S3
	* Only need to retain the most recent copy
	* Occur Async
	* Multi snapshot at volume is allowed
		* 5 pendings for gp2, io1 or magnetic
		* 1 pending for st1 or sc1
	* Can't be deleted when used by AMI
* EBSIOBalance% and EBSByteBalance% help to determine if instances are sized correctly
* Raid 0 or 1’s total performance is caped by the instance limit
* Troubleshooting
	* Instance Limit Exceeded : reached limit on region you can create
	* Insufficient Instance Capacity : AWS has no resource of instance you want
	* Instance Terminates Immediately : reach EBS volume limit, EBS snapshot is corrupted, EBS volume is encrypted and you don’t have permission to the KMS key, AMI is missing a required part ( image.part.xx file )
* You can use CloudWatcn Events with StepFunction to automate created EBS snapshot to DR to to other region
* gp1, st1, sc1 send metric to C.W every 5 mins, io1 sends every 1 min
* Only io1 has VolumeThroughputPercentage & VolumeConsumedReadWriteOPs

# AutoScaling
* Consider the option of creating launch configuration from existing EC2 instance
* If any of the problems appear, you need to re-make the launch configuration and attach to the desired auto scaling group
	* The AMI used for launch configuration could be not existing when creating EC2
	* The instance type chosen might not be available
	* The IAM role might not be existing
	* The S.G configured might not existing
	* The Key Pair configured might not existing
* Launch or terminate EC2 automatically base on
	* Health status check
	* User defined policies
	* Scheduels
	* Manually using set-desired-capacity

# S3
* Bucket must enable versioning to enable MFA delete
	* Bucket owner, root and authorized IAM user can enable versioning
	* Only bucket owner or root can enable MFA delete
* Operations need MFA authentication
	* Change the versioning state of bucket - suspend / reactiviate versioning
	* Permanently delete an object version
* Requirement for MFA delete
	* Root account credential
	* MFA code
* Limitation
	* Only through CLI or API, not console
	* Only root account can do so
* Steps
	* aws s3api list-buckets -> list buckets
	* aws s3api get-bucket-version --bucket \<bucketname\> -> check versioning status
	* aws s3api put-bucket-versioning --bucket \<bucketname\> --versioning-configuration Status=Enabled --mfa \<device serial numbe> -> enable versioning and MFA delete
* Amazon S3 PUT and PUT Object copy operations synchronously store the data across multiple facilities before returning SUCCESS
* S3 also regularly verifies the integrity of data stored using checksums. If Amazon S3 detects data corruption, it is repaired using redundant data
* In addition, S3 calculates checksums on all network traffic to detect corruption of data packets when storing or retrieving data
* Using Server-Side Encryption, S3 encrypts the object <mark>before</mark> saving it on disks in its data centers and decrypt it when the objects are downloaded
* Server-side encryption encrypts <mark>only the object data</mark>. Any object metadata is not encrypted
* S3 handles the encryption (as it writes to disks) and decryption (when you access the objects) of the data objects

# Glacier
* Use lifecycle rules from S3 to glacier
* From on-premises directly to glacier ( how? )
* From EC2 use API to glacier
* Retrieval
	* Expedited 1 - 5 mins
	* Standard 3 - 5 hours
	* Bulk 5 - 12 hours

# CloudHSM
* Supports both symmetric and asymmetric keys
* When a tampering event is detected, the HSM is designed to securely destroy the keys rather than risk compromise
* Symmetric use <mark>A</mark> key and the same key to encrypt and decrypt data : AES-128, AES-256

![CloudHSM](/assets/images/Symmetric-Encryption.png)

* Assymetric use public and private keys to encrypt and decrypt data : RSA, DSA etc

![CloudHSM](/assets/images/Asymmetric-Encryption.png)

* Recommended use at least 2 HSMs in 2 different AZs for production env ; for mission critical at least 3
* Need to set S.G ; In order to talk to CloudHSM, client server needs to install agent
* Handle failover automatically ; will replace failed HSM

# CloudWatch
* Can't aggregate data across region
* Only EC2 with detail monitoring enabled can aggregate statitics
* Metrics keeps for 2 weeks
* Custom metrics
	* Standard resolution data are once per minute
	* High resolution data are once per second
* Namesapce : Groups related metrics together
* Dimension : Name and value pair
* Alarm status
	* OK, Alarm, Insufficient Data
		* Alarm has started, metric not available or insuffecient
* Alarm action
	* SNS
	* Auto scanling
	* EC2 actions
		* Recover this instance
		* Stop this instance
		* Terminate this instance
		* Reboot this instance  
* CloudWatch log
	* Parsing through log to look for particular event to trigger alarm
* Number of evaluation periods for an alarm, multiplied by length of each evaluation period, cannot exceed one day
* Cannot change the name of existing alarm
* Metrics cannot be deleted but expired after 15 month automatically if no new data  is publish to them 
* Data retention
	* 1 minute data points are available for 15 days
	* 5 minute data points are available for 63 days
	* 1 hour data points are available for 455 days

# DirectConnect
* Use direct connect gateway to connect on-premise to multi VPCs in <mark>different</mark> region

![DirectConnect](/assets/images/dx-gateway.png)

# CloudTrail
* Trail log can only save to S3
* Can use log file integrity validation to confirm integrity of log file
	* Built using industry standard algorithms: SHA-256 for hashing and SHA-256 with RSA for digital signing
	* When creating trail, under storage location section, under advanced, with <mark>Enable log file validation</mark> option
* Integrates with CloudWatch logs and events
	* API event occur -> C.T captures and records as event -> view and download from event history -> save event log to S3 -> push event logs to C.W logs and events for further process

# Storage Gateway
* File Gateway
	* Use NFS 3 or 4.1 / SMB 2 or 3
	* Only for Linux
	* Can manage data on S3 such as lifecycle, cross-region replication, version.  Can think file gateway as file system mount on S3
* Volume Gateway
	* Use iSCSI - NTFS/ext4 related
	* Stored
		* Store data locally, async backup point-in-time snapshot to S3
	* Cached
		* Store data on S3, cache frequently used data locally

# Elastic Load Balancer
* Metrics
	* SurgeQueueLength is ELB related, max queue size is 1024
	* SpilloverCount = excess connections that ELB can’t hold ( surgequeuelength can’t hold ) then ELB will reject them
* NLB can use static IP for each AZ for ip whitelisting
* NLB, when registering target group, if use ip, then the source ip = NLB node private ip.  If use ID, then the source ip = real source server ip
* ALB : filter content, route to desired server
* NLB : round robin request to servers, great for serverless architecture
* CLB : use for other situation

# RDS
* RDS databases for e.g. MySQL, Oracle etc. have the data in a single AZ
* Parameter group : Set/group of db engin configuration settings that can be apply to DBs
* Option group : To enable extra features provided by DB engine but not enable in RDS
* Auto backup
	* Snapshot at S3 ; is taken during maintenance window
	* Is deleted when terminating RDS, not manual backups
* During backup, if only one RDS, performance is impact, if is Multi-AZ, backup is on standby
* Retention set to 0 = disable auto backup, max day is 35
* Restoring from backup = creating new RDS, data parameter or S.G is not retented
* When restoring, db engine can change such as standard to enterprise
* When Multi-AZ enabled, manual backup is on standby?
* Read replica can create to other region
* For Multi-AZ, scaling up on standby first, then do failover.  Same as maintainence upgrading
* When using read replica, auto backup is enforced, min is 1 day ; auto backup can't stop when read replica exists
* When read replica exists, can't turn retention period to 0
* RDS uploads transaction logs for DB instances to Amazon S3 every 5 minutes
* Chaging retention day to 0 cause db outage
* Enecryption : only can enable during creation, after that can't alter.  If want to change, can only use copy snapshot and enable encryption, not restore
* Aurora
	* Snapshots (including encrypted ones) can be shared with another AWS accounts 
	* Encryption of existing unencrypted Aurora instance is not supported. Create a new encrypted Aurora instance and migrate the data
	* Automated backups are always enabled on Aurora DB Instances
	* Aurora is designed to transparently handle the loss of up to two copies of data without affecting database write availability and up to three copies without affecting read availability
	* Using SSD-backed virtualized storage layer
	* Restoring a snapshot creates a new Aurora DB instance
	* Based on the database usage, Aurora storage will automatically grow, from 10GB to 64TB in 10GB increments with no impact to database performance
	* Aurora provides data durability and reliability by replicating the database volume six ways across three Availability Zones in a single region
		* Aurora automatically divides the database volume into 10GB segments spread across many disks.
		* Each 10GB chunk of your database volume is replicated six ways, across three Availability Zones
	* If in any case the data is unavailable within Aurora storage,
		* DB Snapshot can be restored or
		* point-in-time restore operation can be performed to a new instance. Latest restorable time for a point-in-time restore operation can be up to 5 minutes in the past

# Billing
* Under consolidating account, RI are shared/charged/used across AZs within region
* If your account is created throught organization, free tier is eligible begins on the day that master account of organization is created
	* Which means if join organization by invite, the link account free tier is preserved (?)
* Can check from Alerts & Notification whether eligible for free tier or not
* From bills section, in the date drop down list, use to know the creation date of your account
* Payer account can be invited and joined payer account? ( should not be )
* Linked accounts receive the cost benefit from other’s Reserved Instances only if instances are launched in the same Availability Zone where the Reserved Instances were purchased

# VPC
* NACL is scanned from top to bottom, first match then stop
* Support inter-region peering
* EC2 talk to internet through IGW if it has public IP, talks to NAT then to IGW if it has private IP
* VPC functions :
	* ![VPC](/assets/images/Xnip2019-03-23_15-50-23.png)
* Subnet functions :
	* ![VPC](/assets/images/Xnip2019-03-23_15-53-50.png)
* By default, all instances in a nondefault VPC receive an unresolvable host name that AWS assigns
* You can assign your own domain name to your instances, and use up to four of your own DNS servers. To do that, you must specify a special set of DHCP options to use with the VPC
* Differences between default and non-default VPC
	* By default, default subnet is public
	* Instance launched into nondefault subnet in default VPC doesn't get public IP or DNS hostname, need to change nondefault subnet setting
	* If new AZ is added in region, AWS auto create new default subnet in the new AZ for you.  If you made changes to default VPC, AWS doesn't add it for you anymore.  You can create your own
* VPC endpoints : S3 & dynamoDB
* VPC gateway : a lot of other services
* If NAT configuration doesn't work
	* Check route table association for NAT instance and gateway
	* Check if source/destination check is disabled for NAT instance
	* Ensure NAT has masquerade configured
	* Restart NAT

![VPC](/assets/images/Xnip2019-04-13_15-00-42.png)
*
![VPC](/assets/images/Xnip2019-04-13_15-02-25.png)



# Networking
![networking](/assets/images/Pasted Graphic 31.png)

![networking](/assets/images/Pasted Graphic 32.png)

![networking](/assets/images/Pasted Graphic 33.png)

# ElastiCache
* Memcached
	* works multi thread
	* 90% CPU is acceptable for memcached
	* Scale up or out for more nodes
	* NOT support auto backup
	* does not support persistence of data
* ElastiCache for Memcached cluster can have
	* Nodes can span across multiple AZs within the same region
	* Maximum of 20 nodes per cluster with a maximum of 100 nodes per region
	* ElasticCache for Memcached supports auto discovery, which enables automatic discovery of cache nodes by clients when they are added to or removed from an ElastiCache cluster
* Mitigating Failures when Running Memcached
	* Mitigating Node Failures
		* spread the cached data over more nodes
		* as Memcached does not support replication, a node failure will always result in some data loss from the cluster
		* having more nodes will reduce the proportion of cache data lost
	* Mitigating Availability Zone Failures
		* locate the nodes in as many availability zones as possible, only the data 		* cached in that AZ is lost, not the data cached in the other AZs
* Redis
	* works single thread
	* CPU usage = cpu utilization (90%) / # of cores ; CPU utilization = 90% / 4 = 22.5%, what we should look at
	* Scale up or out using sharding
	* Support auto backup
	* In cluster form
	* Support up to 16 nodes and up to 5 read replicas, each of up to 3.55 TiB of in-memory data
	* ElastiCache for Redis supports (similar to RDS features)
		* Redis Master/Slave replication.
		* Multi-AZ operation by creating read replicas in another AZ
		* Backup and Restore feature for persistence by snapshotting
		* Can scale up but can't scale down
	* Append Only File (AOF)
		* Provides persistence and can be enabled for recovery scenarios
		* If a node restarts or service crash, Redis will replay the updates from an AOF file, thereby recovering the data lost due to the restart or crash
		* Cannot protect against all failure scenarios, cause if the underlying hardware fails, a new server would be provisioned and the AOF file will no longer be available to recover the data
		* Enabling Redis Multi-AZ as a Better Approach to Fault Tolerance, as failing over to a read replica is much faster than rebuilding the primary from an AOF file
	* Read replica cannot span across regions and may only be provisioned in the same or different AZ of the same Region as the cache node primary
	* ElastiCache for Redis shard consists of a primary and up to 5 read replicas
* Evictions = data store in mem push out ; if is hight, means spec to small
* CurrConnection = Too high number might be application issue, extensive connection
* Automatically detect and replace failed cache nodes, providing a resilient system that mitigates the risk of overloaded databases
* Provides enhanced visibility into key performance metrics associated with the cache nodes through integration with CloudWatch
* ElastiCache currently allows access only from the EC2 network and cannot be accessed from outside networks like on-premises servers
* Mitigating Failures when Running Redis
	* Mitigating Cluster Failures
		* Redis Append Only Files (AOF)
			* enable AOF so whenever data is written to the Redis cluster, a corresponding transaction record is written to a Redis AOF
			* when Redis process restarts, ElastiCache creates a replacement cluster and provisions it and repopulating it with data from AOF
			* It is time consuming
			* AOF can get big
			* Using AOF cannot protect you from all failure scenarios
		* Redis Replication Groups
			* A Redis replication group is comprised of a single primary cluster which your application can both read from and write to, and from 1 to 5 read-only replica clusters.
			* Data written to the primary cluster is also asynchronously updated on the read replica clusters
			* When a Read Replica fails, ElastiCache detects the failure, replaces the instance in the same AZ and synchronizes with the Primary Cluster
			* Redis Multi-AZ with Automatic Failover, ElastiCache detects Primary cluster failure, promotes a read replica with least replication lag to primary
			* Multi-AZ with Auto Failover is disabled, ElastiCache detects Primary cluster failure, creates a new one and syncs the new Primary with one of the existing replicas
	* Mitigating Availability Zone Failures
		* locate the clusters in as many availability zones as possible

![networking](/assets/images/AWS-ElastiCache-Redis-vs-Memcached.png)


# WAF
* Can be deployed on CloudFront or ALB
* WAF rules are managed by AWS and keep up to date

# CloudFormation
* Json and YAML(support comment) form 
* Stack is used to manage a group of related resources as single unit
* Use stack to define all resources that is required to provision a new web application
* you can update stack, for example, to update AMI.  Update behavior
	*  no interupt
	*  with some interupt
	*  replacement
*  Change Sets
	* Summary(preview) of propose changes that will modify/change your current set  
* <mark>Used to deployed AWS services</mark>
* Template create/update/delete stack, stack create/update/delete AWS resources
* When performing update and fails, C.F will roll back the change.  If roll back fails, it shows UPDATE\_ROLLBACK\_FAILED, means stack's resources have been changed outside of C.F
* WaitCondition
	* Only sign back that something is finished, not talking about sequencing
* DependsOn
	* Creation of resources or service installation must happen in order
* cfn-init
	* Use to retrieve and interpret resource metadata, install packages, create files, and start services. 
* cfn-signal
	* Use to signal with a CreationPolicy or WaitCondition, so you can synchronize other resources in the stack when the prerequisite resource or application is ready. 
* cfn-get-metadata
	* Use to retrieve metadata for a resource or path to a specific key. 
* cfn-hup
	* Use to check for updates to metadata and execute custom hooks when changes are detected.
* outputs
	* describe the values that are returned when viewing stack’s properties
* Mapping
	* mapping of keys and associated values used to specify conditional parameter values, similar to lookup table
* Conditons
	* used to control when certain resources are created or whether certain resources properties are assigned a value during stack creation or update
* Parameters
	* values to pass to template runtime when creating or updating stack.  Parameters can be referred from Resources and outputs section of the template
* Resources
	* Specifies the stack resources and properties such as EC2 or S3 buckets
* Transform
	* For serverless application, specifies the version of serverless application mode (SAM) to use.  When you specify a transform, you can use SAM syntax to declare resources in your template

# Kinesis
* Kinesis data stream = take raw source data and pipe to kinesis data analytic OR Spark on EMR or EC2 ( own application ) OR LAMBDA then output with BI
* Kinesis firehose = take raw source data and process it ( by using lambda or glue template ) then saved at S3 OR Redshift OR ELS OR Splunk then output to analytics tools

# DynamoDB
* DynamoDB synchronously replicates data across three facilities in an AWS Region, giving high availability and data durability
* DynamoDB supports fast in-place updates. A numeric attribute can be incremented or decremented in a row using a single API call
* Durability, performance, reliability, and security are built in, with SSD (solid state drive) storage and automatic 3-way replication
* allows provisioned table reads and writes
	* Scale up throughput when needed
	* Scale down throughput four times per UTC calendar day
* automatically partitions, reallocates and re-partitions the data and provisions additional server capacity as the
	* table size grows or
	* provisioned throughput is increased
* Global Secondary indexes (GSI) can be created upfront or added later
* Fine Grained Access Control (FGAC) gives a high degree of control over data in the table
* FGAC helps control who (caller) can access which items or attributes of the table and perform what actions (read/write capability).
* FGAC is integrated with IAM, which manages the security credentials and the associated permissions
* Encryption at rest can be enabled only for a new table and not for an existing table
* Encryption once enabled for a table, cannot be disabled
* DynamoDB Streams do not support encryption
* On-Demand Backups of encrypted DynamoDB tables are encrypted using S3’s Server-Side Encryption
* Keep item size small
* Store metadata in DynamoDB and large BLOBs in Amazon S3
* Use table per day, week, month etc for storing time series data
* DynamoDB supports Partition Key & Partition Key and Sort Key
* Use conditional or Optimistic Concurrency Control (OCC) updates
* Supports Cross-region Replication
* Supports global tables, needs stream to enable
	* multi-region, multi-master
	* Global Tables ensures eventual consistency
	* Global Tables replicates data among regions within a single AWS account, and currently does not support cross account access
	* replica tables and secondary indexes in your global table have identical write capacity settings to ensure proper replication of data
* Streams
	* DynamoDB Streams provides a time-ordered sequence of item-level changes made to data in a table in the last 24 hours, after which they are erased
	* DynamoDB Streams maintains ordered sequence of the events per item however across item are not maintained

* DynamoDB Secondary indexes
	* add flexibility to the queries, without impacting performance.
	* are automatically maintained as sparse objects, items will only appear in an index if they exist in the table on which the index is defined making queries against an index very efficient
* ElastiCache can be used in front of DynamoDB in order to offload high amount of reads for non frequently changed data
* DynamodB DAX is a fully managed, highly available, in-memory cache for DynamoDB that delivers up to a 10x performance improvement
* One read request unit = one strongly consistent read request or two eventually consistent read requests, for an item up to 4 KB in size
* Transactional read requests require 2 read request units to perform one read for items up to 4 KB
* Conclusion :
	* 1 read request unit = 1 strong consistent read requst to handle 4KB size data
	* 1 read request unit = 2 eventually consistent read request to handle 4 KB * 2 = 8 KB size data
	* 2 read request units = 1 transactional read request to handle 4KB size data
* Example for 8KB item size :
	* 2 read request unit for strong consistent read
	* 1 read request unit for eventually consistent read
	* 4 read request unit for transactional read
* One write request unit represents one write for an item up to 1 KB in size
* Transactional write requests require 2 write request units to perform one write for items up to 1 KB
* Conclusion :
	* 1 write request unit = 1KB data size
	* 2 write request units = 1 transactional write request to handle 1 KB data size
* Example for 2 KB item size :
	* 2 write request units
	* 4 write request units for 2 transactional requests

# Elastic Transcoder
* Convert video and audio files into supported output formats
* Source and output location can be S3
* Use transcoding pipeline to connect input (S3) and to output (S3)
* Use transcoding job to transcode media file

# Data Pipeline
* AWS Data Pipeline is a web service that you can use to automate the movement and transformation of data. With AWS Data Pipeline, you can define data-driven workflows, so that tasks can be dependent on the successful completion of previous tasks. You define the parameters of your data transformations and AWS Data Pipeline enforces the logic that you've set up.

# Elastic Beanstalk
* <mark>Most convenient way to deploy application on AWS</mark>
* What's the different between Web Server and Worker?
* Handle deployment details automatically
	* Capacity provisioning
	* Load balancing
	* Automatic scaling
	* Application health monitoring

# EFS
* Regional based service

# Policies and Permissions
* Policy types
	* Indentity based policies
	* Resource-based policies
	* AWS organization service control policies (SCPs)
	* Access control lists (ACL)

![policies](/assets/images/Pasted Graphic 26.png)

* Switch role
	* When switching role, only one set of permission is active ( IAM user or role ), when switching to role, you temporarily give up your IAM permission
	* You can only switch roles when you sign in as an IAM user. You <mark>cannot</mark> switch roles if you sign in as the AWS account root user
	* Policy can be attached to User, Group and role
* OrganizationAccountAccessRole is the default role name created in member account when creating member account from organizaiton

# Organization
* Enables you to
	* Centrally manage policies across multiple AWS accounts
	* Control access to AWS services
	* Automate AWS account creation and management
	* Consolidate billing across multiple AWS accounts
* Consolidated billing is a feature of AWS Organizations
* Max 5 levels to OUs
* Organization permissions overrule account permissions
* When another service is configured and authorized to access with the organization, AWS Organizations creates an <mark>IAM service-linked role</mark> for that service in each member account
* All accounts in an organization automatically have a service-linked role created, which enables the AWS Organizations service to create the service-linked roles required by AWS services for which you enable trusted access
* These additional service-linked roles come with policies that enable the specified service to perform only those required tasks
* AWS Organizations is <mark>eventually consistent<mark>
* AWS Organizations achieves high availability by replicating data across multiple servers in AWS data centers within its region
* When creating an account in your organization, Organizations automatically creates a root user and an IAM role for the account.
* This role has full administrative permissions in the member account.
* The role can only be switch by payer account by default, setting is under role -> Trust relationships -> Trusted entities
* To access the member accounts using the role, you must sign in as a <mark>user from the master account that has permissions to assume the role</mark>
* The user must have policy with following policy
	* Service : STS
	* Action : AssumeRole
	* ARN : arn:aws:iam::accountIdNumber:role/rolename
* Root
	* Parent container for all the accounts for the organization.
	* Policy applied to the root is applied to all the organizational units (OUs) and accounts in the organization.
	* There can be only one root currently and AWS Organization automatically creates it when an organization is created
* OU
	* A policy attached to one of the nodes in the hierarchy, flows down and affects all branches (OUs) and leaves (accounts) beneath it
	* An OU can have exactly one parent, and currently each account can be a member of exactly one OU
* Service Control Policy
	* SCPs override member account IAM permission policy
* SCP can be attached to
	* A root, which affects all accounts in the organization
	* An OU, which affects all accounts in that OU and all accounts in any OUs in that OU subtree
	* An individual account
* Master account of the organization is not affected by any SCPs that are attached either to it or to any root or OU the master account might be in
* If you replace the default policy on the root, all accounts in the organization are affected by the restrictions

# IAM
* IAM supports IdPs that are compatible with OpenID Connect (OIDC) or SAML 2.0 (Security Assertion Markup Language 2.0)

# GuardDuty
* Protects AWS accounts and workloads
* Monitors your AWS environment for suspicious activity and generates findings
* Allow you to add your own threat list and trusted IP lists

![guardduty](/assets/images/Pasted Graphic 27.png)

# Trusted Advisor
* Reduce Costs, Increase Performance, and Improve Security
* Scan your infrastructure against AWS managed best practise policies and provide recommendations
	* Yellow : Investigation recommended
	* Red : Action recommended
* Available for
	1. Cost Optimization
	2. Performance
	3. Fault Tolerance
	4. Service Limits (available for free support plan)
	5. Security (available for free support plan)
		* Check for S.G settings
* All AWS customers get access to the seven core Trusted Advisor checks to help increase the security and performance
	1. S3 Bucket Permissions
	2. Security Groups - Specific Ports Unrestricted
	3. IAM Use
	4. MFA on Root Account
	5. EBS Public Snapshots
	6. RDS Public Snapshots
	7. Service Limits

![policies](/assets/images/Pasted Graphic 40.png)

# Config
* Continusly track resources inventory and changes aginst config rules
* Report current system status or configuration of resources
	* Knowledge about current configuration
* Control the configuration history
	* Understanding of configuration change history
* Have visibility into possible human errors and control change management
	* Capability to monitor configuration change
* Config <mark>DOES NOT</mark> show <mark>WHO</mark> make the changes!
* AWS Config rule represents your desired configuration settings for specific AWS resources or for an entire AWS account
* Monitor and record the configuration of AWS resources.  Save the files in S3 and send SNS when configuration is created, modified and deleted
* Config is designed to oversee application resources
* To exercise better governance over your resource configurations and to detect resource misconfigurations
* Capable of providing historical configuration of resources
* Show relationships between resources related to particular configuration
* help to analysis security issues easier such as when was what permission was granted to which user OR when was certain S.G role was modified
* Config does the followings
	* Evaluate your AWS resource configurations for desired settings.
	* Get a snapshot of the current configurations of the supported resources that are associated with your AWS account. 
	* Retrieve configurations of one or more resources that exist in your account.
	* Retrieve historical configurations of one or more resources.
	* Receive a notification whenever a resource is created, modified, or deleted.
	* View relationships between resources. For example, you might want to find all resources that use a particular security group
* Resources
	* services used such as EC2, S.G, VPC, ELB etc
* Configuration History
	* Configuration of particular resource in the past.  Configuration history file is sent to S3 automatically by resource
* Configuration Item
	* point-in-time view of all attributes of a resource which includes metadata, attributes, relationships, current configuration, and related events.  Configuration Item is created whenever a change to the monitored resources is detected.
* Configuration Recorder
	* Stores the configurations of all the resources as Configuration Item.  Must first create and start C.R before recording.  C.R can be created customized
* Configuration Snapshot
	* Collection of configuration items of all supported resources
* Configuration Stream
	* an automatically updated list of all configuration items for the resources that AWS Config is recording, Every time a resource is created, modified, or deleted, AWS Config creates a configuration item and adds to the configuration stream then notify by SNS
* Resources Relationship
	* AWS Config discovers AWS resources in your account and then creates a map of relationships between AWS resources
* Trigger for config rule
	* When configuration changes ( when the configurations stored in C.R is detecting changed )
	* Periodic time
* Multi-Account Multi-Region Data Aggregation
	* Source Accoun
		* A source account is the AWS account from which you want to aggregate AWS Config resource configuration and compliance data
	* Source Region
		* A source region is the AWS region from which you want to aggregate AWS Config configuration and compliance data
	* Aggregator
		* Resource type that collects config configuration and compliant data from multiple source account and region.  Aggregator is created in the region where you want to see the aggregated data
	* Aggregator Account
		* An aggregator account is an account where you create an aggregator.
	* Authorization
		* As a source account owner, authorization refers to the permissions you grant to an aggregator account and region to collect your AWS Config configuration and compliance data. Authorization is not required if you are aggregating source accounts that are part of AWS Organizations
* How AWS Config Works
	1. First discover all the supported resources then generates a configuration item for each resources
	2. New C.I is generated when resource is changed
	3. Historical C.i changes are kept
		- For example, removing an egress rule from a VPC security group causes AWS Config to invoke a Describe API call on the security group. AWS Config then invokes a Describe API call on all of the instances associated with the security group. The updated configurations of the security group (the resource) and of each instance (the related resources) are recorded as configuration items and delivered in a configuration stream to an Amazon Simple Storage Service (Amazon S3) bucket
	4. Config continuously monitor resource configuration against rule, when a change is detected, Config will flag the resource as noncompliant and send SNS.  Each config rule is associated with and lambda function.

# Compute Optimized Instances
* Ideal for compute-bound applications that benefit from high-performance processors.
	* Batch processing workloads
	* Media transcoding
	* High-performance web servers
	* High-performance computing (HPC)
	* Scientific modeling
	* Dedicated gaming servers and adserving engines
	* Machine learning inference and other compute-intensive applications
* General Purpose Instances
	* General purpose instances provide a balance of compute, memory, and networking resources, and can be used for a variety of workloads.
* Memory Optimized Instances
	* Memory optimized instances are designed to deliver fast performance for workloads that process large data sets in memory.
* Storage Optimized Instances
	* Storage optimized instances are designed for workloads that require high, sequential read and write access to very large data sets on local storage. They are optimized to deliver tens of thousands of low-latency, random I/O operations per second (IOPS) to applications



# Inspector
* Works on instance level
* Only inspector can check for S.G

# T.A VS Config VS Inspector
* Config reads CloudTrail logs and does two things:
	1. It creates a timeline of changes made to tracked resources (so you can see how a resource like an S3 bucket has been modified over time)
	2. It allows you to create rules to detect whether your environment is in compliance with certain policies (e.g. all your EBS volumes are encrypted). You can send notifications or take automated action with Lambda when a resource violates a rule.
* Trusted Advisor is there to help customers follow general AWS best practices. E.g. are your EC2 instances overprovisioned and you're wasting money as a result? Are you approaching any service limits? Is there MFA on root?
* Inspector provides an agent that goes onto EC2 instances. It provides packaged rules to check for unintended network accessibility of your Amazon EC2 instances and for vulnerabilities on those EC2 instances. It can check the patch level of the OS.

# System Manager
* EC2Rescue
	* EC2Config - suitable before 2016
		* detach volume, attach to another instance, modify configuration file at \Program Files\Amazon\Ec2ConfigService\Settings\config.xml, change EC2SetPassword to Enable, attach back to instance, restart, use private key to obtain new administrator password from console
	* EC2Launch - suitable after 2016
		* detach volume, attach to another instance, download EC2Rescue for Windows Server zip file, execute EC2Eescue.exe and use Rest Administrator Password function, attach back to original instance, use private key to obtain new administrator password from console
* Automation
	* pre-defined documents that user can used to perform jobs or action to resources by providing necessary values or information to the templates
	* pre-defined documents that can do many things, need to provide necessary variables.  Can execute at once, rate control. across account and region, manual execution.  Used to execute on resources in the account
	* Has document, action, queue
		* Document defines action
		* Action includes one or more steps which each step is a particular action or plugin
		* Action defines inputs, behavior and outputs
		* Queue is the place to hold excess automation that an AWS account can execute.  An AWS account can execute 25 automations simultaneously.  75 queues max per AWS account 
	* Can be executed across multiple regions and accounts or OUs
* Run Command
	* pre-defined documents with logs write to S3 or send SNS.  Need to fill in vairables depends on the document chose to run
	* Need to run on servers with SSM agent.  Choose to run on number of instances and how many to run concurrently
* State Manager
	* Similar to Run Command but different at, state manager can set schedule ( like cron ) so can repeatedly checking/setting the selected server at desired state ( chosen document state ).  Result is written to S3

![policies](/assets/images/Pasted Graphic 44.png)

# Step Functions
![policies](/assets/images/Pasted Graphic 45.png)

# OpsWork
* <mark>Used to deply application stack</mark>
* AWS OpsWorks Stacks uses Amazon CloudWatch to provide thirteen custom metrics with detailed monitoring for each instance in the stack.
* AWS OpsWorks Stacks integrates with AWS CloudTrail to log every AWS OpsWorks Stacks API call and store the data in an Amazon S3 bucket.
* You can use Amazon CloudWatch Logs to monitor your stack's system, application, and custom logs
* AWS OpsWorks Stacks use puppet
	* Let you configure AWS managed puppet enterprise master server in AWS
	* Provides full-stack automation by handling tasks such as software and operating system configurations, package installations, database setups, change management, policy enforcement, monitoring, and quality assurance
	* Master server is backup at time you choose
	* Can use auto scaling groups to associate new EC2 nodes with server automatically
	* Master server manages configuration of nodes by using puppet-agent in nodes
	* Before can launch master server, must have <mark>git repositiory</mark> to store and change-manage of puppet modules and classes
	* Need attach AWSCodeCommitReadOnly policy to server
	* SSH is not necessary or recommended for master, can use console or CLI
	* Need to set network and S.G for master server, can't change after setting
	* Server goes offline during maintainence, so choose low server demand time within office hour
	* Auto backup enable by default, set frequency and backup time.  Daily is minimum time can choose, max 30 backups in S3
	* Manual backups are not included in 30 auto backups limitation; 10 backups most, need manual deletion excessing backups
	* When creating master server, before it is online, download Starter Kit and the Puppet Enterprise console credentials, only show one time, after that, no more
	* To work with master server and add nodes to manage, you need to install certificate can can only do by CLI
	* To use puppet api, must create short-term token, default lifetime is 5 mins, max is 10 yrs
	* Recommended way to add node is use associateNode() API
	* Master server maintenance can delete agent information, so recommended to install manually ( something like this )
	* Can associate nodes with your Puppet master automatically by populating EC2 instance user data
	* By default, after ten sign-in attempts, users are locked out of the Puppet console
	* System maintenance is required a minimum of once a week
	* System maintenance deletes any files or custom configuration that you have added to the OpsWorks for Puppet Enterprise server
	* Restore server from AMI with custome files to recover them
	* system maintenance on demand can only execute by CLI, not console
	* Disassociate or remove managed node can be done by CLI or Puppet Enterprise console, NOT OpsWorks for Puppet Enterprise management console
	* Puppet enterprise API now can only disassociate 1 node a time
	* Recommend to disassociate nodes from master server first then delete master server
	* Delete master server also delete all auto backups BUT NOT manual backups
	* Server creation fails with "requested configuration is currently not supported" message
		* Cause: An unsupported instance type might have been specified for the Puppet master
		* Solution: If you choose a VPC that has a non-default tenancy, use an m4 instance type, which can support dedicated tenancy
	* Unable to create the server's Amazon EC2 instance
		* Cause: This is most likely because the EC2 instance doesn’t have network access.
		* Solution: Ensure the instance has outbound Internet access, and the AWS service agent is able to issue commands. Be sure that your VPC (a VPC with a single public subnet) has DNS resolution enabled, and that your subnet has the Auto-assign Public IP setting enabled
	* Service role error prevents server creation
		* Cause: This can occur when the service role you are using lacks adequate permissions to create a new server
		* Solution: Open the OpsWorks for Puppet Enterprise console; use the console to generate a new service role and an instance profile role. If you would prefer to use your own service role, attach the AWSOpsWorksCMServiceRole policy to the role. Verify that opsworks-cm.amazonaws.com is listed among services in the role's Trust Relationships. Verify that the service role that is associated with the Puppet master has theAWSOpsWorksCMServerRole managed policy attached
	* Elastic IP address limit exceeded
		* Cause: This occurs when your account has used the maximum number of Elastic IP (EIP) addresses. The default EIP address limit is five.
		* Solution: You can either release existing EIP addresses or delete ones that your account is not actively using, or you can contact AWS Customer Support to increase the limit of EIP addresses that is associated with your account
	* Unattended node association fails
		* Cause: This can occur when you do not have an IAM role set up as an instance profile that permits opsworks-cm API calls to communicate with new EC2 instances.
		* Solution: Attach a policy to your EC2 instance profile that allows the AssociateNode andDescribeNodeAssociationStatus API calls to work with EC2, as described in Adding Nodes Automatically in OpsWorks for Puppet Enterprise
* AWS OpsWorks Stacks
	* Stacks > Layers > Chef recipes > tasks
	* Layers use chef recipes to handle tasks
	* Doesn't require or need to create Chef Server like OpsWork for Chef
	* Monitor and provision instances for your and auto healing and auto scaling when necessary
	* Stack : Container for AWS resources
	* Layer : Stack is constructed by adding one or more layers
	* Recipes : Layers depend on Chef recipes to handle tasks
	* Lifecycle event : Run specified recipes at particular lifecycle events
	* Five lifecycle events
		* Setup : Occurs on a new instance after it successfully boots
		* Configure : Occurs on all of the stack's instances when an instance enters or leaves the online state
		* Deploy : Occurs when you deploy an app
		* Undeploy : Occurs when you delete an app
		* Shutdown : Occurs when you stop an instance
	* Typecal procedure, after an instance finish boots
		* Run the layers setup recipes, for necessary packages installation
		* Run the layers deploy recipes, to install application
		* Run the Configure recipes on every instance in the stack to adjuest itself to accommodate the new instance
	* Setup
		* Once a new instance has booted, OpsWorks triggers the Setup event, which runs recipes to set up the instance according to the layer configuration for e.g. installation of apache, PHP packages
		* Once setup is complete, AWS OpsWorks triggers a Deploy event, which runs recipes to deploy your application to the new instance.
	* Configure
		* Whenever an instance enters or leaves the online state, AWS OpsWorks triggers a Configure event on all instances in the stack.
		* Event runs each layer’s configure recipes to update configuration to reflect the current set of online instances for e.g. the HAProxy layer’s Configure recipes can modify the load balancer configuration to reflect any added or removed application server instances.
	* Deploy
		* OpsWorks triggers a Deploy event when the Deploy command is executed, to deploy the application to a set of application servers.
		* Event runs recipes on the application servers to deploy application and any related files from its repository to the layer’s instances.
	* Undeploy
		* OpsWorks triggers an Undeploy event when an app is deleted or  Undeploy command is executed to remove an app from a set of application servers.
		* Event runs recipes to remove all application versions and perform any additional cleanup tasks.
	* Shutdown
		* OpsWorks triggers a Shutdown event when an instance is being shut down, but before the underlying EC2 instance is actually terminated.
		* Event runs recipes to perform cleanup tasks such as shutting down services.
OpsWorks allows Shutdown recipes a configurable amount of time to perform their tasks, and then terminates the instance
	* Each layer can have multiple recipes assigned to them
	* OpsWorks Stacks create instances and adds them to a layer
	* After the EC2 instance has finished booting, OpsWorks Stacks installs an agent that handles communication between the instance and the service and runs the appropriate recipes in response to lifecycle events
	* If an instance is belong to different layers, OpsWorks runs the recipes for each layer
	* Linux based computing resources created outside of the OpsWorks stacks for e.g. console or CLI can be added, incorporated and controlled through OpsWorks
	* OpsWorks integrates with IAM for
		* How individual users can interact with each stack, such as whether they can create stack resources such as layers and instances, or whether they can use SSH or RDP to connect to a stack's Amazon EC2 instances
		* How AWS OpsWorks Stacks can act on your behalf to interact with AWS resources such as Amazon EC2 instances
		* How apps that run on AWS OpsWorks Stacks instances can access AWS resources such as Amazon S3 buckets
		* How to manage users' public SSH keys and RDP passwords and connect to an instance
	* Stack can't be cloned from region to region
	* Resources can be managed only in the region in which they are created
	* Works with 13 detailed CloudWatch metrics
	* Works with CloudTrail for all api tracking and store in S3
	* Works with CloudWatch logs to monitor stack's system, appliation and custom logs
	* AWS OpsWorks Stacks provides three ways to manage the number of server instances.
		* 24/7 instances are started manually and run until they are manually stopped. 
		* Time-based instances are automatically started and stopped by AWS OpsWorks Stacks on a user-specified schedule. 
		* Load-based instances are automatically started and stopped by AWS OpsWorks Stacks when they cross a threshold for a user-specified load metric such as CPU or memory utilization
	* Recommendation: If you are managing stacks with more than a few application server instances, we recommend using a mix of all three instance types
	* Each stack has a Permissions page that you can use to grant users permission to access the stack and specify what actions they can take
	* You specify a user's permissions by setting one of the following permissions levels. Each level represents an IAM policy that grants permissions for a standard set of actions.
	* Deny denies permission to interact with the stack in any way. 
	* Show grants permissions view the stack configuration but not modify the stack state in any way. 
	* Deploy includes the Show permissions and also grants the user permissions to deploy apps. 
	* Manage includes the Deploy permissions and also allows the user to perform a variety of stack management actions, such as creating or deleting instances and layers
	* If an agent does not communicate with the service for more than approximately five minutes, AWS OpsWorks Stacks considers the instance to have failed
	* Auto healing is set at the layer level, An instance can be a member of multiple layers. If any of those layers has auto healing disabled, AWS OpsWorks Stacks does not heal the instance if it fails
	* If a layer has auto healing enabled—the default setting—AWS OpsWorks Stacks automatically replaces the layer's failed instances as follows:
		* Instance store-backed instance
			1. Stops the Amazon EC2 instance, and verifies that it has shut down. 
			2. Deletes the data on the root volume.
			3. Creates a new Amazon EC2 instance with the same host name, configuration, and layer membership. 
			4. Reattaches any Amazon EBS volumes, including volumes that were attached after the old instance was originally started. 
			5. Assigns a new public and private IP Address.
			6. If the old instance was associated with an Elastic IP address, associates the new instance with the same IP address.
		* Amazon EBS-backed instance
			1. Stops the Amazon EC2 instance, and verifies that it has stopped.
			2. Starts the EC2 instance
	* You use the Elastic Load Balancing console or API to create a load balancer and then attach it to an existing layer
	* To use Elastic Load Balancing with a stack, you must first create one or more load balancers in the same region by using the Elastic Load Balancing console, CLI, or API. You should be aware of the following:
		* You can attach only one load balancer to a layer.
		* Each load balancer can handle only one layer.
		* AWS OpsWorks Stacks <mark>does not</mark> support Application Load Balancer. You can only use Classic Load Balancer with AWS OpsWorks Stacks
	* You cannot attach a load balancer when you create a custom layer. You must edit the layer's settings
	* An Amazon RDS service layer represents an Amazon RDS instance. The layer can represent only existing Amazon RDS instances, which you must create separately by using the Amazon RDS console or API
		1. Use the Amazon RDS console, API, or CLI to create an instance.Be sure to record the instance's ID, master user name, master password, and database name. 
		2. To add an Amazon RDS layer to your stack, register the Amazon RDS instance with the stack. 
		3. Attach the layer to an app, which adds the Amazon RDS instance's connection information to the app's deploy attributes
	* Stacks can also be run in VPC to be isolated from direct user interaction
	* Separate Stacks can be created for different environments like Dev, QA etc
	* Custom recipes and related files is packaged in one or more cookbooks and stored in a cookbook repository such S3 or Git
* OpsWorks Stacks
	* Automatic instance scaling and auto healing
* OpsWorks for Chef Automate
	* Dynamically configures newly provisioned instance with auto scaling groups
* OpsWorks for Puppet
	* Use auto scaling to register and provision new nodes

# Extra
* Create Autoscaling Group out of running EC2
* CloudWatch Events co-work with Lambda functions
* Disk Queue Depth & Free stroage Space
* Use RDS console to check for maintenance status
* Use parameter section in CloudFormation template to specify SSM parameter which contains the latest necessary information
* AWS provides windows AMI notification, subscript to SNS and trigger Lambda function to update CloudFormation template
* Use CloudWatch logs to monitor Opswork stack's system, application and custom logs
* Enable MFA for the IAM users
* On demand backup and enable point in time recovery for dynamoDB
* VPC flow logs for ENI for connection information
* Send ENI to CloudWatch logs for data transfer and bandwidth
* S3 use bucket policy to limit read, write and delete permission
* Route table to VGW ( virtual private gateway ) to connect to on-premises network
* Create DHCP option set for making instance in VPC to use on-premise DNS server
* Set DeleteOnTermination to False
* VPN connection is dual tunnel, so can setup secondary VPC connection
* Aurora DDL can alter table with no impact to db
* developer plan has 5x8 to cloud support associate via mail, the rest is 7x24 via more than mail
* Use exponential backoff between createStack api calls for CloudFormation
* Monitor CPUUtilization, SwapUsage, Evictions, CurrConnections of ElastiCache
* In VPC, private route with NAT is main route, custom route is with IGW
* Use VPC Flow logs to get ELB detailed traffic
* Use S3 lifecycle policy to maintain the same file name
* CloudTrail log to S3, S3 use object-create event to trigger lambda.  CloudTrail information is sent to lambda, so to filter out particular action and trigger SNS when necessary
* Consolidating billing is part of Organization service
* When EC2 warm period is not expired yet, metric is not aggregrated
* Congito use IAM role to control signed in user permisson
* Storage file gateway use NFS
* Detailed monitoring needs to enable for aggregating statistics
* Can aggregate metric within region, can't across region.  Metric is separate between region
* For custom metric, use custom namespace
* S3 delete with MFA, enable by root and cli only
* Create IAM role with session duration parameter, from 900 seconds (15 minutes) up to the Maximum CLI/API session duration setting for the role
	* If you specify a value for the DurationSeconds parameter that is higher than the maximum setting, the operation fails

![guardduty](/assets/images/Xnip2019-04-13_22-36-33.png)
*
![guardduty](/assets/images/Xnip2019-04-13_22-36-45.png)
*
![guardduty](/assets/images/Xnip2019-04-13_23-29-03.png)
*
![guardduty](/assets/images/Xnip2019-04-07_00-20-18.png)
*
![guardduty](/assets/images/Xnip2019-04-07_00-21-28.png)
*
![guardduty](/assets/images/Xnip2019-04-07_00-22-08.png)