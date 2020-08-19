---
title: AWS Self Learning Part2
author: bulafish
date: 2018-07-18 +0800
categories: [Study Note]
# tags: []
---

# AWS core services - compute
1. Core services
  * foundation to all solution
  * EC2


### Computer Services Overview
1. Computer services overview
  * EC2  
  * Lambda
  * Auto scaling
  * ELB
  * Elastic beanstalk
    * PaaS service that quickly deploy, scale, manage your application
  * Lightsail, is PaaS
  * ECS - elastic container service  
    * Scalable, high-performance
    * Container management service
    * Support docker container
    * Run on managed cluster of EC2
  * EKS - elastic container service for kubernetes
    * Managed service
    * Provide Kubernates clusters service
  * Fargate
    * Technology for ECS & EKS allow user to run containers without managing servers or clusters
    * Interface to run containers on ECS & EKS
    * Not to worry / think about manage servers or cluster


### Introduction to EC2
1. EC2
  * Elastic
  * Properly pre-configured
  * Able to increase or decrease the amount according to application's demand automatically
  * Provides IPv6 support in VPC environment


2. AMI
  * Teplate for the root volume for the instance
  * Specify which volume to attach to the instance when launched
  * Defines the OS, initial software and state of patches that will be on an instance  


3. AMI life cycle  
  * Create, automatic registered, ready for use
  * Deregister when no longer needed
  * Create EC2 with AMI
  * Can be copied within or to different regions
  * Can be transfer to other AWS account
  * Able to launch EC2 with AMI not belong to you if the owner grants you launch permission


4. EC2 pricing
  * On demand
  * Spot instances
  * RI
  * Dedicated hosts
  * Linux & Ubuntu based instances
    * Charged by second with minimum of 60 seconds    
  * Other OSs are charged by hour
    * Minimum charged is hour


5. EC2 costs
  * On demand, spot instances, RI
    * Per second billing
  * Dedicated hosts
    * Per hour billing


6. Review EC2 pricing options use cases
  * Detailed cloud watch provides `seven pre-selected` metrics for fixed monthly rate


7. Spot instance hibernation
  * Hibernate EBS backed instance when interrupted
  * Resume instance when capacity is available
  * Hibernation agent required
  * Hibernated spot instance can only be resumed by spot service
  * Use encrypted EBS for root volume is recommended
    * Instance memory is stored on root volume during hibernation


### EC2 Cost Optimization
1. Cost optimization
  * Pay only for what you need and when you need


2. Four pillars of cost optimization
  * Right-sizing
    * Process of finding opportunities to downsize the deployed resources when possible
  * RI-reserved instances
  * Elasticity
  * Monitor and improve


3. Right size
  * Right size then reserve


4. RI
  * Standard - only size can be modified during the term
  * Convertible - can be exchanged for another convertible RI instance with new attirbutes


5. Dedicated instance
  * Isolate instance at hardware level
  * Meet BYOL model that the hardware is dedicated to one tenant


6. RI- reserved instances
  * Fixed compute power can be used on different instances
  * Reserved a m4.large
    * 08:00 ~ 17:00, used for instance A
    * 17:00 ~ 08:00, used for instance B
    * Must be in same AZ


7. Consolidation billing
  * Multiple AWS accounts joined benefits the discount for total volume used


8. AWS trusted advisor
  * Provides real time guidance to help provision resources following AWS best practices


9. AWS cost explorer
  * Free tool use to view graphs of your costs for up to the last 13 months
  * Forecast how much are likely to spend for the next three months


{% include ads3.html %}


### Introduction to lambda
1. Lambda
  * Full managed and scales automatically service
  * High availability
  * Maximum of 5 mins
  * Even driven service
  * Charged on the times code is triggered
  * each 1/100 millisecond of exec time
  * Go, NodeJS, Java, C#, and Python


### Introduction to elastic beanstalk
1. Elastic beanstalk  
  * Packer Builder, Single Container / Multicontainer or Preconfigured Docker, Go, JavaSE, Java with Tomcat, .NET on Windows Server with IIS, Node.js, PHP, Python, and Ruby
  * Apache, Nginx, Passenger, and IIS
  * is PaaS


2. Setup elastic beanstalk
  * Choose instance type
  * Choose database type
  * Set and adjust auto scaling
  * Upload / update application
  * Access server log files
  * Enable HTTPS on ELB


# AWS core services - Storage

### EBS
* EBS volumes are off-instance storage
* Analogous(類似) to virtual disks in the cloud


1. Temporary storage
  * Instance store or ephemeral storage


2. Persistent storage (non volatile storage)  
  * EBS (volume)
    * 99.999 % availability, about 5 minutes down time per year
    * Annual failure rate is about 0.1% to 0.2% (AFR)
    * Mountable, only to EC2 in same AZ
    * Block level storage
    * Auto replicate within its AZ
    * Backup instance into AMI and store in S3
    * Elastic volumes (change volume size when needed)
    * Off instance storage (virtual/cloud hdd)
    * Can only attach to one EC2 at one time
    * Attach directly to EC2
  * S3
    * Accessible from anywhere
    * Object level storage
    * 5MB to 5 TB per object maximum


3. Block VS Object storage
  * Block is typically faster and use less bandwidth
  * Block cost more than object
  * Block storage only modified/change the data block
  * Object storage change the whole file regardless of change size


4. EBS volume types
  * SSD
    * General purpose (GP2)
      * Max Size: 1GB ~ 16TB
      * Max IOPS/Volume: 10,000
      * Max Throughput/Volume: 160 Mib/s
      * Max IOPS/Instance: 80,000
      * Max Throughput/Instance: 1,750MB/s      
      * Performance Attribute: IOPS
    * Provisioned IOPS (IO1)
      * Max Size: 4GB ~ 16TB
      * Max IOPS/Volume: 32,000
      * Max Throughput/Volume: 500 Mib/s
      * Max IOPS/Instance: 80,000
      * Max Throughput/Instance: 1,750MB/s      
      * Performance Attribute: IOPS
  * HDD
    * Throughput-Opt (ST1)
      * Max Size: 500GB ~ 16TB
      * Max IOPS/Volume: 500
      * Max Throughput/Volume: 500 Mib/s
      * Max IOPS/Instance: 80,000
      * Max Throughput/Instance: 1,750MB/s      
      * Performance Attribute: MB/s
    * cold (SC1)
      * Max Size: 500GB ~ 16TB
      * Max IOPS: 250
      * Max Throughput: 250 Mib/s
      * Max IOPS/Instance: 80,000
      * Max Throughput/Instance: 1,750MB/s      
      * Performance Attribute: MB/s


5. EBS volume types - use case
  * Encrypted volume can only be attached to instance support types
  * SSD
    * General purpose
      * Recommended for most workloads
      * System boot volumes
      * Virtual desktops
      * Low latency interactive apps
      * Development and test environments
    * Provisioned IOPS
      * I/O intensive workloads
      * Relational DBs
      * NoSQL DBs
  * HDD
    * Throughput-Opt
      * Streaming workloads requiring consistent, fast Throughput at a low price
      * Big data
      * Data warehouses
      * Log processing
      * Cannot be a boot volume
    * cold
      * Throughput-oriented storage for large volumes of data that's infrequently accessed
      * Scenarios where the lowest storage cost is important      
      * Cannot be a boot volume


6. Snapshot
  * Incremental backups
  * Point in time
  * Encrypted (volume) at no cost
  * Copy or share between regions
  * Image stored at S3, charge at per GB per month
  * Snapshot transfer to S3 cost money?
  * Volume size or used data size store in S3?
    * Only used data size is store in S3
  * Only pay for incremental pieces, not the full storage
  * Data is `lazily loaded` in the background to volume when creating from snapshot
    * Meaning volume is available right away but data is put into volume from snapshot at S3 when needed

7. EBS charges
  * By the amount provisioned per month
  * By second, 60 seconds minimum
  * Boot volume
    * General purpose SSD
      * By the amount provision in GB per month
    * Magnetic
      * By the number of request to volume
    * Provisioned IOPS SSD
      * By the amount of IOPS provisioned (% of day / month used)
  * Extra volume
    * Above plus
      * Throughput Optimized HDD
      * Cold HDD


### S3
1. S3
  * Object level storage
  * Store data in bucket
  * Re-upload the entire modified file if just changed a part of a file
  * Support cross-region replication, need to enable versioning first


2. Peview
  * Fully managed, scale seamlessly
  * Eleven 9s of durability, data stored redundantly
  * Bucket name must be universal unique
  * Object upload or delete can trigger post actions
  * Data in transit or at rest can be encrypted
  * S3 analytics analyze storage access patterns and transition the right data to right storage class
  * Size per object is from 0 Bytes to 5TB
  * Single put max size is 5GB
  * Use multi-part upload if object is larger than 100 MB
  * Can be accessed through virtual private cloud endpoints
  * Provides `identity`, `access management policies`, `bucket policies` and `per-object access control lists` for access control
  * Private by default
  * Use S3 analytic to identify optimal life-cycle policy
    * Move less access data to S3 standard-IA
    * Policy to monitor entire bucket, prefix or object tag


3. S3 storage classes
  * Standard
    * Frequently accessed data
    * Store data in minimum of `three AZs`
  * Standard-IA
    * Less frequently accessed data but requires rapid access when needed
    * Store data in minimum of `three AZs`
  * One zone-IA
    * Same as Standard-IA but store data in `single AZ`
  * Glacier
    * Store data in minimum of `three AZs`
  * Support cross-region replication, if enabled
  * Encryption needs to enable manually
    * Encrypted with server-side Encryption (SSE-S3)
    * Encrypted each object with a unique key
    * Encrytion key is encrypted with master key which regularly rotates
    * Use AES-256 (Advanced Encryption Standard) to encrypt data
  * KMS-Key Management System
    * Use customer master keys (CMKs) to encrypt S3 objects
    * SSE-C (customer-provided server-side Encryption) allow user to use own encryption key.  S3 does the encryption and decryption


4. S3 Cost
  * Pay for
    * GBs per month, number and size of objects stored
    * Transfer out to other regions
    * PUT, COPY, POST, LIST, GET requests
  * Not pay for
    * Transfer in to S3
    * Transfer out to cloudfront or EC2 in the same region


5. S3 Pricing
  * Standard Storage
    * Eleven 9s durability
    * 99.99% availability
  * Standard-Infrequent Access (SIA)
    * Eleven 9s durability
    * 99.9% availability
  * Get charges at different rate than other requests such as PUT and COPY


### EFS-Elastic File System
1. EFS preview
  * Fully managed service
  * Deployed to VPC, then select subnets
  * Access by public internet
  * FW need to open tcp port 2049 (NFSv4, version 4.0 and 4.1)
  * Need to allow tcp 2049 port bi-direction between EC2 and EFS
  * Shared storage
  * Elastic capacity
  * Accessed by region, across-AZs
  * Need to create mount target for EC2s to mount


2. EFS resources
  * In EFS, file system is the primary resource
    * ID
    * Creation token
    * Creation time
    * File system size in bytes
    * Number of mount targets created for the file system
    * File system state
  * Mount target
    * Mount target ID
    * Subnet ID belonged to
    * File system ID belonged to
    * IP address of the file system to be mounted
    * Mount target state


### Glacier
1. Glacier Preview
  * Data archiving service
  * Eleven 9s durability
  * Support SSL/TLS encryption of data in transit and at rest
  * Use vault lock to forbidden vault alter


2. Glacier
  * Object Storage
  * Replicate automatic to AZs
  * Data encrypted by default
  * Cost lower than S3
  * Hours to retrieve data when needed
  * Ideal for archiving
  * Archive
    * Objects stored, is the base unit
    * 1 byte to 40 terabytes
    * Has its unique ID and description
  * Vault
    * Specify vault name and region when creating
    * Container to store archives
  * Vault Access Policy
    * Authorization & authentication    
    * One per vault
  * Vault lock
    * Prevent vault being altered
    * One per vault
  * Retrieving Data
    * Expedited
      * Within 1 ~ 5 minutes, highest cost
    * Standard
      * Within 3 ~ 5 hours
    * Bulk retrievals
      * within 5 ~ 12 hours, lowest cost
  * Console only provide few operations
    * Creating, deleting vaults
    * Creating, managing archive policy
    * Rest of operations are accessible by REST API, AWS Java or .NET SDKs
    * Able to archive data into Glacier using S3 lifecycle policies
  * Cost more to retrieve data from Glacier then S3
    * Cost per retrieved is higher than S3
    * Cost per GB retrieved is higher than S3
  * Enable control access to data by using IAM
  * Pay for
    * Storage per GB
    * per upload (how exactly is charged)
    * Free tier for download, after that will occur cost


3. Glacier Controls
  * Web console
    * Creating, deleting vaults
    * Managing archive policies
  * The rest controls need to be done by CLI or SDK or API


4. Life Cycle
  * Can be set on per `object` or per `bucket`


5. S3 VS Glacier

|S3|Glacier
-|-|-|
Data volume|No limit|No limit
Average latency|ms|min/hrs
Item size|5 TB max|40 TB max
Cost/GB per month|$$|$
Billed requests|PUT, COPY, POST, LIST, GET|UPLOAD and retrieval
Retrieval pricing|$ per request|$$ per request and per GB
Encrytion|Option|Default


6. Server-side encryption
  * Server-side encryption (SSE-S3)
    * Encrypt every object with `unique key`
    * encrypt unique key with `master key`
    * Master key rotate regularly
  * AWS key management service (AWS KMS)
    * Use customer's master key (CMKs) to create encryption key
    * From KMS encryption keys section or in IAM or via AWS KMS APIs
    * Use encryption key to encrypt objects
  * Server-side encryption with customer provided encryption key (SSE-C)
    * Allow customer to set his own encryption keys


7. Glacier Security
  * Control access with IAM
  * Encrypted objects with AES-256
  * Manages your keys for you


# VPC-Virtual Private Cloud


### VPC
1. VPC
  * Allow customer to provision virtual private network within AWS cloud
  * Build on AWS global infrastructure of regions and AZ
  * Only live within a single region
  * Logically isolated virtual network, control of
    * IP address ranges
    * Subnet creation
    * Route table creation
    * Network gateways
    * Security settings
  * Biggest CIDR is /16 and smallest is /28
  * You can define
    * IP address ranges
    * Subnet creation
    * Route table creation
    * Network gateways
    * security settings


2. VPC deployment
  * Able to isolate subnet
  * Define access control list
  * Customizing routing table
  * VPC exists in all AZs of the region once created


3. VPC Features
  * Define IP ranges for VPC
  * Divide VPC into subnets
    * Subnet must set IP range within VPC ip range created
    * Subnet must select AZ within the region
    * Public or private subnet can be created
  * NAT gatway can be EC2 instance or hosted service
  * By default, all subnets within a VPC can communicate with each other
  * VPC can span multiple AZs but subnet cannot
    * VPC exists in multiple AZs
    * Subnet exists only in particular AZ
  * Classific as public, private, vpn only
  * Default VPC has one public subnet in every AZ within region with netmask of /20


4. VPC Address
  * Address can't be changed after set
  * Biggest and smallest CIDR is /16 and /28


5. VPC components
  * Subnets
    * Don't span AZs
    * Classific as public, private, vpn only
    * Default VPC has one public subnet in every AZ within region with netmask of /20
    * First 4 and last IP, total of 5, are reserved by AWS
  * Route tables
    * Used to control traffic going out of the subnets
    * Each subnet must be associated with a route table and only a route table at one time    
  * DHCP, two options
    * Domain-name-servers
    * domain-name
  * Security group
    * Virtual `stateful` firewall
  * ACLs-Network Access Control Lists
    * Control in and out traffic of one or more subnets, `stateless`
  * IGW-Internet Gateway
    * Allow internet access from VPC
    * horizontally scaled, redundant, and highly available
  * EIP-Elastic IP Address
  * ENI-Elastic Network Interface
    * Virtaul network interface can be attached to instance
  * Endpoints
    * Private direct connection between VPC and other AWS service
  * Peering
    * Allowing two VPCs to communicate
  * NAT-Network Address Translation (NAT Gateway OR NAT Instance)
    * Accepts, translates and forwards traffic within a private subnet


6. VPC Connections
  * AWS hardware VPN
    * IPsec hardware VPN connection between VPC and your remote network
  * AWS direct connect
    * Dedicate private connection from your remote network to VPC
  * AWS VPN cloudhub
    * multiple aws hardsare vpns via your vpn to enable communicate between various remote networks
  * Software VPN
    * Connect your remote network by building VPN software on EC2


### VPC Security Groups
  1. VPC securities
  * Routing table
  * Subnet - ACLs - `stateless`
    * Keeps track of the state of interaction
  * Isolating subnets
  * Security group - `stateful`
    * No information is retained by either sender or receiver
    * Request handled entirely base on the information comes with it


### CloudFront
1. CloudFront
  * CDN with both network and application level
  * Programmable
  * Set origin as S3 or ELB, only pay for storage, not data transferred between


2. Cost estimation
  * Traffic distribution
    * pricing base on edge location where content is served
  * Request
    * Number and type of http/https requests made AND geographic region where the request is made
  * Data transfer out
    * Amount of data transfer out from edge location to client


# Database


### RDS
1. Currently Support
  * MySQL
  * Aurora
  * MsSQL
  * PostgreSQL
  * MariaDB
  * Oracle


2. High availability with multiple AZs
  * Provide multi-AZ deployment
  * Generate standby copy in another AZ within same VPC
  * `Synchronously` replicate data from Standard to standby
  * Auto bring standby to become master when master failed
  * Application connect to RDS by DNS, so no interruption will occur


3. Read replicas
  * `Asynchronous` replication
  * Promote to master if needed `manually`
  * Suitable for read heavy workloads
  * Offload read queries
  * Read replicas can be created in `different region` than master database


4. When to use RDS
  * Massive read/write rates (150K write/second)
  * Sharding due to high data size or throughput demands
  * Simple GET/PUT requests and queries that NoSQL can handle
  * RDBMS customization


5. Costs
  * Pay per hour
  * Pay by DB specs
  * Pay by purchase types (On demand, RI)
  * Pay by number of DBs (multiple DBs to handle peak loads)
  * Charge for terminated backup storage per GB/month
  * No charge for backup storage of active DB
  * Charge for additional backup storage to the provisioned storage at per GB/month
  * Number of input and output request made to database
  * Storage and I/O charges vary depending on `single AZ` or `multiple AZs`
    * Storage and I/O charges vary, depending on the number of Availability Zones you deploy to
  * Tired charges for outbound data transfer


### DynamoDB
1. What is DynamoDB
  * virtually unlimited storage
  * May have differing attributes
  * Scalable read/write throughput
  * Store data across multiple facilities
  * Partition data and has table storage to meet workload automatically
  * No limitation on number of items per table
  * Items in same table may have different attributes
  * Auto scale storage to meeting heavier loading
  * Provision amount of read and write throughput you need for your table
  * storage table can scale automatically or manually with provision read/write throughput


2. Core components
  * Tables
    * Collation of data
  * Items
    * Group of attributes that is uniquely identifiable among all of the other items
  * Attributes
    * Fundamental data element, something that does not need to be broken down
  any further
  * Partition key
    * Simple primary key, composed of one attribute called the partition key
  * Partition key & sort key
    * Also known as composite primary key which is composed of two
  attributes
  * Table data is partitioned and indexed by primary key
  * Find item by
    * Primary key
      * More efficient as table data are partitioned by primary key
    * Scan
      * Flexibility to locate item by other non-attributes
      * Less efficient as DB needs to scan all items in table to make your find


3. Items in table must have a key
  * Single key
    * EX. GID, UID, etc
  * Compound key
    * EX. author + book title


4. Overview
  * Run exclusively on SSDs
  * Support document and key-value store models
  * Global tables features replicates tables to your choice of regions automatically


### Redshitf
* Data warehouse service
* Use for analytic application
* Run complex analytic queries of structured data
* Using sophisticated query optimization, columnar storage on high performance local disk
* Massive parallel query execution


1. Parallel processing
  * Work flow
    * Communicate between application and computer node
    * Develop execution plans to carry out DB operation
    * Separate the plan to elements and pass to computer nodes for execution
    * Computer send result back to leader node for aggregation after execution


2. Automation and scaling
  * Scalability provided, adjust capacity manually
  * Data is encrypted by default both at rest and in transit


3. Compatibility


4. Use cases
  * Use for agility
  * Free from hardware and expertise problems with low cost
  * Free from DB admin works
  * Scalable SaaS


5. Review
  * Scale with no down time by adding more nodes
  * Automatically adds nodes to cluster and redistributes data for max performance
  * Use columnar storage and massively parallel processing architecture
  * To parallelize and distribute data and queries across multiple nodes
  * Provide high performances consistently
  * Monitor and backup automatically
  * built in encryption, need to turn on manually

### Aurora
* MySQL & PostgreSQL compatible relational DB
* performance 5 times faster then MySQL
* Drop in compatibility with MySQL 5.6 using Inno db storage engine
* Integrates with db migration service and schema conversion tool


1. High availability
  * High availability and resilient design over MySQL
  * Storing `six copies` across `three AZs` with continuous backups to `S3`
  * Up to `fifteen read` replicas can be configured
  * Design for instant crash recovery when primary DB is unhealthy


2. Resilient design
  * Perform redo log on every read operation
  * Reduce restart time after db crash to less than 60 secs in most cases
  * Move buffer cache out of db process so is available right at restart time
  * Prevents you from having to throttle access until the cache is repopulated to avoid brownouts


3. Review
  * High performance and scalability
    * Provide 5X throughput of MySQL and 3X throughput of PostgreSQL
  * High availability and durability
    * Provide 99.99% availability
    * Fault-tolerant and self-healing storage built for
    * Six replicas of data store across three AZs
    * And continuously backup to S3
  * Multiple levels of security
    * Isolation by VPC
    * Encrypted at rest using keys you created and control by KMS
    * Encrypted in transit with SSL
  * Compatible with MySQL and PostgreSQL
  * Fully managed


# AWS core services - ELB, CloudWatch & Auto Scaling


### ELB
* distributes incoming traffic across EC2, containers and IP addresses


1. Types of ELB
  * ALB-Application
    * HTTP & HTTPS
    * Operate at layer 7
    * Support content-basing routing
    * Application in containers
    * Support websocket and http/2
    * additional visibility into health of target instance and container
  * NLB-Network
    * TCP
    * Operate at layer 4
  * CLB-Classic
    * previous generation for HTTP, HTTPS & TCP


2. ELB use cases
  * To secure access to web server through a single exposed point
  * Decouple environment using `internet` / `intranet` facing LB
  * Provide high availability and fault tolerance
  * Ability to distribute traffic across multiple AZs
  * Increase elasticity and scalability with minimal overhead


3. ALB features
  * Path and host based routing
  * Support IPv6
  * Dynamic ports
  * Additional request protocols supported
  * Deleting protection and request tracking
  * Enhanced metrics and access logs
  * Targeted health checks


4. ALB use cases
  * Ability to use container to host your micro services and route to applications
  * Allow you to route different request to same instance but differ the path based on the port
  * Set routing rules to distribute traffic to only the desired backend application if having different containers listening various ports


5. NLB use cases
  * Optimized for sudden and volatile traffic patterns
  * Single static IP per AZ
  * Ultra-low latencies, ideal for application require extreme performance


6. ELB monitoring
  * See HTTP responses
  * Number of healthy and unhealthy hosts
  * Filter metrics base on AZ or LB


7. Review
  * LB is used to increase capacity and reliability of application
  * ALB - suitable for HTTP and HTTPS traffic and advanced routing function, including micro services and containers
  * NLB - suitable for TCP traffic where extreme performance is required
  * CLB - Across multiple EC2 and operating at request and connection level


### CloudWatch (CW)
1. What is CloudWatch?
  * Distributed statistics gathering system
  * Collect and track your metrics from your application
  * Can create your own metric
  * Enable you to track and monitor performance and health of resources and application
  * Use CW to collect and monitor log files from EC2, CloudTrail, R53 and others
  * Basic CW - `free`
    * `Seven` pre-selected metrics at `five minutes` interval
    * `Three` status check metrics at `one minutes` interval
  * Detailed CW - `Paid`
    * All metrics available to basic monitoring at `one minutes` interval
    * Allow data aggregation by EC2 AMI ID and instance type
  * Retain metrics for 15 months at no charge
  * CW support three retention schedules
    * 1 minute data points are available for 15 days
    * 5 minute data points are available for 63 days
    * 1 hour data points are available for 455 days


2. CloudWatch terms
  * Three main components
  * Metric
    * Data point from monitored resources or applications.
  * Alarm
    * Send notification when tracked metric reach a specified value for a specified period of time
    * Sent by SMS, email(SES?), or notify auto scaling policy to take action
  * Event
    * Monitor resources and deliver near real time stream of events describe the changes in resources
    * Stream of resource changes can be sent to other AWS resources


3. CloudWatch Actions
  * Stop, terminate, reboot, recover an instance
  * Scale and auto scaling group in or out
  * Send message to SNS topic


### Auto Scaling
1. What is scaling?
  * Scaling plans for
    * EC2 instances and spot fleets
    * DynamoDB tables
    * DynamoDB tables and indexes
    * Aurora replicas


2. Scaling
  * Scaling in or out on specified conditions
  * Register new instances to LB automatically when specified
  * Can launch across AZs


3. Auto Scaling Components
  * Launch Configuration (What)
    * AMI
    * Instance type
    * Security groups
    * Roles
  * Auto Scaling Group (Where)
    * VPC and subnets
    * LB
    * Min instances
    * Max instances
    * Desired capacity
      * Number of instances you wish to start with
  * Auto Scaling Policy (When)
    * Scheduled
    * Conditioned
    * On demand
    * Scale out policy
    * Scale in policy
  * Steps
    * First, create a launch configuration
    * Next, create an automatic scaling group
    * Then define at least one automatic scaling policy
