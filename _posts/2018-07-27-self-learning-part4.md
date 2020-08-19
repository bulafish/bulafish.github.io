---
title: AWS Self Learning Part4
author: bulafish
date: 2018-07-27 +0800
categories: [Study Note]
# tags: []
---

# Cloud Architecting

### Well-Architected Framework
1. Pillars of well-architected framework
  * Operational Excellence
  * Security
  * Reliablity
  * Performance Efficiency
  * Cost Optimization


2. Operation Excellence
  * Ability to run and monitor systems
  * Improve supporting process and procedure continually
  * Manage and automate changes
  * Respond to event and changes
  * Achieve all above to delivery business value


3. Security
  * Ability to protect information, systems, assets
  * Delivering business value through risk assessments and mitigation strategies
  * Five areas of cloud Security
    * IAM ; Authentication and authorization
    * Detective controls ; Identify threats by log analyzing or auditing controls
    * Infra protection ; Network ACLs, ssh-key, OS patching etc
    * Data protection ; Backup, replication, recovery, encryption etc
    * Incident response ; SOP to mitigate and response to potential security incidents


4. Security Design Principle
  * Security applied everywhere, at all layers
  * Enable traceability through logging and auditing
  * Principle of least privileges, grant minimum privileges needed
  * Secure system such as OS, application, data etc
  * Automate all processes, respond to all security incidents or auto create security harden image and use it when needed


5. Reliability, the ability of a system to
  * Recover from infra or service failures
  * Acquire more resources to meet demand dynamically when needed
  * Mitigate disruptions


6. Reliability Areas
  * Foundations
    * Well-designed foundations capable of handling changes in demand and to auto heal itself when failure detected
  * Management Changes
    * Fully understand how changes affect your system and make plan accordingly to adjust it quickly and reliably
  * Failure Management
    * Anticipate, aware of, respond to and prevent failures from happening
    * Take advantage of automation with monitoring
    * Swap out fault systems first to maintain reliable, troubleshoot failed system later


7. Reliability Design Principles
  * Test recovery procedures
  * Automatically recover
  * Scale horizontally
  * Stop guessing capacity
  * Manage changes in automation


{% include ads3.html %}


8. Performances Efficiency
  * Use computing resources efficiently to meet system requirements
  * Maintain that efficiency as demand changes and technologies evolve
  * Four Keys
    * Selection customizable solutions
    * Review to continually innovate
    * Monitor AWS services
    * Consider the trade-offs


9. Performance Design Principles
  * Democratize advanced technologies
  * Go global in minutes
  * Use a serverless architecture
  * Experiment more often
  * Have mechanical sympathy
    * Use technology approach that best aligns to what you want to achieve


10. Cost Optimization
  * Avoid or eliminate unneeded cost
  * Suboptimal resources
  * For elements
    * Use cost-effective resources
      * Use appropriate services, resources and configuration
    * Match supply with demand
    * Increase expenditure awareness
      * Breakdown current cost, predict future costs and plan accordingly
    * Optimize over time


11. Cost Optimization Design Principles
  * Adopt a consumption model
  * Measure overall efficiency
    * Measure the business output of the systems and costs associated with delivering it
  * Reduce spending on data center operations
  * Analyze and attribute expenditure
  * Use managed services


### Well-Architected Design Principles
1. Well-Architected Design Principles
  * Stop guessing your capacity needs
    * Scale up/down or in/out automatically
  * Test systems at production scale
    * Duplicate live system, test at production scale, delete all after done
  * Automate to make architectural experimentation easier
  * Allow for evolutionary architectures
  * Drive architectures using data
    * Using data/statistics to deploy/modify/decide/improve architecture rather than guess or assumption
  * Improve through game days


2. Reliability (可靠性)
  * Measure of how long a resource performs its intended function
  * Probability that `entire system` function for a specified period of time
  * A measure of how long the item performs its intended function
  * Measure by
    * Mean time between failure - total time in service / number of failures
    * Failure Rate - Number of failures / total time in service


3. Availability (可用性)
  * Percentage of time resources are operating normally
  * Normal operation time / total time
  * Measure of the percentage of time the resources are in an operable state
  * Amount of time your system is in a functioning condition
  * Improving availability usually leads to increased cost


4. High Availability Factors
  * Fault tolerance (容錯性)
    * Build-in redundancy
  * Recoverability
    * Policies, process, procedures related to restoring service
  * Scalability
    * Ability to accommodate growth, part of application's availability


5. HA on AWS
  * Multiple servers
  * Isolated, redundant data centers within each Availability Zone
  * Multiple Availability Zones within each Region
  * Multiple Regions across the globe
  * Fault tolerant services to use as you please


6. Benefits of cloud over data center
  * Trade capital expense for variable expense -> Stop buying hardware.
  * Benefit from massive economies of scale -> Benefit from Amazon’s purchasing
  power.
  * Eliminate guessing on your capacity needs -> Construct a flexible, highly
  available solution using scaling.
  * Increase speed and agility -> Deploy and decommission with just a few clicks.
  * Stop spending money to run and maintain data centers -> Purchase only the
  services needed.
  * Go global in minutes
