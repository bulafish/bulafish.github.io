---
title: AWS Self Learning Part5
author: bulafish
date: 2018-07-30 +0800
categories: [Study Note]
# tags: []
---

### AWS Organization
1. Introduction
  * Account management service to consolidate multiple AWS accounts into an organization
  * Benefits
    * Centrally manage access policies across multiple AWS accounts
    * Control access to AWS services
    * Automate AWS account creation and management
    * Consolidate billing across multiple AWS accounts
  * Root -> OU -> AWS accounts
  * A OU can contain OUs
  * Policy affects all of the branches and leaves when attach to the node
  * An OU can have only one parent
  * An account must be a member of exactly one OU


2. Features and benefits
  * Create service control policies (SCPs) control AWS services across multiple AWS accounts centrally
  * Create groups, add accounts into group, apply policy to group
  * simplify account management by using APIs
  * Setup a single payment method for all AWS accounts in organization.  With consolidated billing, combined view of charges incurred by all accounts is provided and is charged with aggregated usage (volume discounts)


3. Security
  * IAM is policy to apply to IAM users, groups or roles to restrict access to AWS services, root account is not affect
  * Service control policies (SCPs) is used to allow or deny particular AWS services on individual or group of AWS accounts.  It affect all IAM users, groups, roles and even root account


4. Steps to setup organization
  * Step 1 - Create your organization with your current AWS account as the master account. You also invite one AWS account to join your organization, and create another account as a member account
  * Step 2 - Next, you create two organizational units (OUs) in your new organization and place the member accounts in those OUs
  * Step 3 - In the third step, you create Service Control Policies (SCPs) which allows you to apply restrictions to what actions can be delegated to users and roles in the member accounts by using service control policies (SCPs). An SCP is a type of organization control policy
  * Step 4 - In the fourth step you test your organization’s policies. You can sign in as users from each of the test accounts and see the affects that the SCPs have on the accounts


5. Ways to access organization
  * Web console
  * CLI
  * SDK
  * HTTPS query API with credentials


{% include ads3.html %}


### Billing and Cost Management
1. Introduction
  * Is a service used to pay bill, monitor usage and budget costs
  * Forecast to get better idea of cost and usage may look like in future
  * Range of time is customizable to view predict cost


2. Billing dashboard
  * Graph on left is past, current and future predict cost
  * Graph on right is current monthly cost break down into services


3. Tools
  * Cost explorer is a free tool that
    * View charts of your costs
    * View cost data for the past 13 months
    * Forecast how much you are likely to spend over the next three months
    * To discover patterns in how much you spend on AWS resources over time - and identify (cost) problem areas
    * Identify which services you use the most and/or metrics like which  Availability Zones have the most traffic or which linked AWS account is used the most
  * Forecast and track costs
    * Use data provided by cost explorer to show the status of your budgets and provide forecast of estimated cost
    * Able to use budgets to create notifications when cost go over budget for the month or when estimated costs exceed budget, using email or SNS
    * Budget can be tracked monthly, quarterly or yearly and time period is customizable
  * Cost and usage reporting
    * Single location for accessing comprehensive infortion about costs and usage
    * Lists usage for each service in hourly or daily line items including tax paid if any
    * Able to pubish billing reports to S3.  Report is updated once a day


### Technical Support Plans and Costs
1. AWS support
  * Proactive guidance
    * Technical account manager (TAM)
  * Best practices
    * Trusted advisor
  * Account assistance
    * AWS support concierge


2. Support plans
  * Basic Support Plan
    * 24/7 access to customer service, documentation, white papers and support forums
    * Access to six core Trusted Advisor checks
    * Access to Personal Health Dashboard
  * Developer Support Plan
    * Want access to guidance and technical support
    * Are exploring how to quickly put AWS to work
    * Use AWS for non-production workloads or applications  
  * Business Support Plan
    * Run one or more applications in production environments
    * Have multiple services activated, or use key services extensively
    * Depend on their business solutions to be available, scalable, and secure
  * Enterprise Support Plan
    * Focus on proactive management to increase efficiency and availability
    * Build and operate workloads following AWS best practices
    * Leverage AWS expertise to support launches and migrations
    * A Technical Account Manager (TAM) provides technical expertise for the full range of AWS services and obtains a detailed understanding of your use case and technology architecture. The TAM is the primary point of contact for ongoing support needs


3. Support plan price

|Basic|Developer|Business|Enterprise
-|-|-|-|-|
Cost|Free|Greater of $29 `OR` <br> 3% of monthly AWS usage|Greater of $100 `OR` <br> 10% of monthly AWS usage for the first $0–$10K <br> 7% of monthly AWS usage from $10K–$80K <br> 5% of monthly AWS usage from $80K–$250K <br> 3% of monthly AWS usage over $250K|Greater of $15,000 `OR` <br> 10% of monthly AWS usage for the first $0–$150K <br> 7% of monthly AWS usage from $150K–$500K <br> 5% of monthly AWS usage from $500K–$1M <br> 3% of monthly AWS usage over $1M
Tech Support | | Business hour access via email | 24x7, email, chat & phone | 24x7, email, chat & phone
TAM | NO | NO | NO | YES
Who can open case? | No one | 1 person / unlimited case | Unlimited contact / Unlimited case | Unlimited contact / Unlimited case


4. Case severity & response times

|Critical|Urgent|High|Normal|low
-|
Basic|No support|No support|No support|No support|No support
Developer Plan(business hour)||||12 hours or less|24 hours or less
Business Plan(24x7)||1hour or less|4 hours or less|12 hours or less|24 hours or less
Enterprise Plan(24x7)|15 minutes or less|1hour or less|4 hours or less|12 hours or less|24 hours


5. Trusted advisor
  * Online resource to
    * Reduce cost
    * Increase performance
    * Improve security
  * Core checks and recommendations to all customers
  * Full trusted advisor benefits available with business or enterprise support plans
  * Found in Management tools section of web console
  * Two types of trusted advisor options
    * Core checks and recommendations default to all accounts
    * Full trusted advisor for business and enterprise support plans
