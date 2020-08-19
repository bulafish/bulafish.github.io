---
title: AWS Self Learning Part1
author: bulafish
date: 2018-07-17 +0800
categories: [Study Note]
# tags: []
---

# Cloud Concepts Overview


### What is cloud computing
1. Cloud service
  * fully management service
  * via internet


2. Cloud computing
  * on-demand
  * via internet
  * pay as you go


3. Cloud computing primary categories
  * IaaS
  * PaaS
  * SaaS


4. Cloud deployment models
  * All in cloud
  * Hybrid
  * Private cloud (On premises)


All in cloud VS On premises

All in cloud| On premises
----|----
No upfront investment|Large initial purchase
Low ongoing costs|Labor, patches, and upgrade cycles
Focus on innovation|Systems administration
Flexible capacity|Fixed capacity
Speed and agility|Long procurement cycle and setup
Global reach on demand|Limited geographic regions


5. Cloud terminology
  * High availability (可用性)
    * Available when you need it
  * Fault tolerance (容錯力)
    * Remain functional with certain level of errors existing
  * scalability (擴展性)
    * Ability to scale up easily when demand increased
  * Elasticity (彈性)
    * Ability to grow but to reduce in size when necessary


{% include ads3.html %}


### Six benefits of cloud computing
1. Advantages
  * Trade Capital Expense for Variable Expense
  * Benefit from massive economies of scale, for lower usage price
  * Eliminate predicting capacity
  * Increase speed and agility
  * Stop spending money running and maintaining data centers  
  * Go global in minutes


### What is AWS
1. What is web service?
  * Software available over internet
  * Use standardized format (XML or JSON) for request and response
  * An API interaction
  * Not tie to any OS
  * Is discoverable


2. AWS by category
  * Core services
    * Infrastructure services such as compute, networking, storage, applications, databases, and analytics
  * Foundational services
    * Cloud-based solutions for the analytics, enterprise, mobile, and IoT platforms
  * Developer and operation services
    * Enable developers and IT operations professionals practicing DevOps to rapidly and safely deliver software


### AWS cloud adoption framework (CAF)
1. Help organizations develop efficient and effective plans for cloud adoption

2. Six core perspectives
  * Business
  * People
  * Governance
  * Platform
  * Security
  * Operations


# Cloud Economics


### Fundamentals of aws pricing
1. AWS fundamentals
  * Pay for
    * Compute, Storage, Data transfer out
    * Data transfer in and between services in the same region is free
    * Aggregated data transfer and charge at transfer rate
      * EC2, S3, RDS, SimpleDB, SQS, SNS, VPC


2. Pricing model
  * Pay per use
  * Pay less when you reserve
    * Save up to 75%
    * AURI, PURI, NURI
  * Pay less when you use more
    * Tiered pricing
    * S3, EBS, EFS
  * Pay even less as AWS grows
  * Custom pricing is available
    * For high-volume projects with unique requirements


3. Consolidated billing
  * Consolidate payment for multiple AWS or AISPL accounts for combined usage for tiered benefits


### Total cost of ownership
1. What is TCO-Total cost of ownership
  * Total cost of an asset plus cost to operate it
  * Total cost of purchasing and operating a technology product during its useful life
  * `Financial estimate intended to help buyers and owners determine the direct and indirect costs of a product or system`


2. On premise VS On cloud
  * On premises IT is a discussion about
    * Capital expenditure
    * Long planning cycles
    * Multiple components to buy, build, manage, and refresh over time
    * Staffs hiring with special or specific skill needed
  * On cloud is a discussion about
    * Flexibility
    * Agility
    * Consumption based costs

3. TCO is used for comparing the costs of running an entire infrastructure environment for a specific workload in an on-premises or co-location facility to the same workload running on a cloud-based infrastructure

4. ROI analysis over cloud to premises
  * Hard benefits
    * Reduced spending on compute, storage, networking, security
    * Avoidance of hardware and software purchases (CapEx)
    * Reductions in operational costs, backup, DR
    * Reduction in operations personnel
  * Soft benefits
    * Reuse of service and application that allow you define, redefine solutions using the same cloud service
    * Increase develop productivity
    * Improved customer satisfaction
    * Improved employee morale
    * Agile business process able to quickly respond to new and emerging opportunities
    * Increase global reach


# AWS Infrastructure


### Global Infrastructure
1. Global infrastructure breaks into
  * Regions
  * Availability zones
  * Edge locations
  * Data centers < Availability zones < Regions


2. Data centers
  * Foundation of AWS infrastructure
  * Build in cluster  
  * Redundant design for fault tolerance
  * Critical system components are backed up accross AZs to ensure availability
  * Deploy infrastructure to support availability with continuously monitoring serivce usage
  * Customer data traffic is redirected automatically in case of failure
  * 50,000 ~ 80,000 physical servers per data center
  * Are all online, no data center is cold
  * Custom network equipment
    * Multi-ODM sourced network equipment
    * Custom network protocal stack


3. Region
  * Geographical location
  * Consist two or more physically separated and isolated AZs
  * Isolated from one another to achieve fault tolerance and stability  
  * Data replication across regions are off by default  


4. Availability zone
  * Physically separated and isolated
  * Consist of one or more discrete data center
  * Design for fault isolation
  * Build redundantly for fault tolerance
  * Connected to other AZ in the same region using private links
  * Redundantly connected to multiple tier-1 transit providers
  * Equipped with uninterruptible power supplies, cooling equipment, backup generators and security to ensure uninterrupted operations


5. Edge locations
  * Where users access AWS services
  * Used for CDN, Route53, Shield and WAF
  * Regional edge caches
    * Between source server and edge location
    * Used for infrequently access contents, S3 excluded


6. Infrastructure features
  * Elastic and scalable
  * Fault tolerant with redundancy
  * high availability with minimal down time
  * minimal to `no human intervention`
  * Provides
    * Compute
    * Networking
    * Storage
    * Database
