---
title: AWS Self Learning Part3
author: bulafish
date: 2018-07-19 +0800
categories: [Study Note]
# tags: []
---

# AWS Cloud Security


### Shared Responsibility Model
* Shared responsibility model changes depending on the AWS services the customer use

1. AWS Security Responsibilities: Security `OF` the Cloud
  * Physical security of data centers
    * Nondescript facilities, 24/7 security guards, two-factor authentication, access logging and review, video surveillance, and disk degaussing and destruction
  * Hardware and software infrstructure
    * Servers, storage devices, and other appliances that AWS services rely on
  * Network infrastructure
    * Host operating systems, service applications, and virtualization software
  * Virtualization infrastructure
    * Routers, switches, load balancers, firewalls, cabling etc


2. Managed services
  * AWS is responsible for the security configuration of its products that are considered foundational or managed services
  * AWS responsible
    * OS and database patching
    * Firewall configuration
    * Disaster recovery
  * Customer
    * Logical access controls
    * Protect account credentials
  * Inherited controls
    * Controls that a customer fully inherits from AWS, such as physical and environmental controls
  * Shared controls
    * Patch Management – AWS is responsible for patching and fixing flaws within the infrastructure, but customers are responsible for patching their guest OS and  applications
    * Configuration Management – AWS maintains the configuration of its infrastructure devices, but customers are responsible for configuring their own guest  operating systems, databases, and applications
    * Awareness and Training - AWS trains AWS employees, but a customer must train their own employees
  * Customer specific
    * Service and Communications Protection or Zone Security, which may  require a customer to route or zone data within specific security  environments


3. Customer security responsibility: Security `IN` the Cloud
  * Instance operating system
    * Including patching, maintenance
  * Application
    * Passwords, role-based access
  * Security groups
  * OS/host-based firewalls
    * Including intrusion detection/preventioin systems
  * Network configuration
  * Account management
    * Separation of access


### Identity and Access Management (IAM)
* IAM is globally set, it applies across all regions
* Authentication first, then authorization
* IAM allows you to set `authentication` and `authorization` to AWS services
  * Authentication - verify identity
  * Authorization - what resources and operations are allowed


1. IAM
  * Manage access and authentication of users centrally
  * Free service
  * Create user, group, roles and attach policies to it to control access
  * Manage what resources(services) can be accessed by who and how they can be accessed
  * Define required credentials base on context


2. Types of security credentials
  * Email address and password
    * Associated with AWS root account
  * IAM user name and password
    * Used for accessing the AWS management console
  * Access and security account keys
    * Used with CLI and programmatic requests like APIs and SDKs
  * Multi-factor authentication
    * Extra layer of security, used for root account and IAM users
  * Key pairs
    * Used for specific AWS services like EC2


3. IAM: Authorization
  * All permissions are implicitly denied by default
    * Unless allowed, otherwise all is denied
  * If something is explicitly denied, it can never be allowed
    * Something explicitly denied is always denied


4. MFA
  * Hardware devices
  * Virtual MFA-compliant applications


5. IAM users
  * Entity crated in AWS
  * A way to interact with AWS
  * No default security credentials for IAM users
    * Have to assign them specifically
  * IAM users are not necessary people


6. IAM group
  * Collection of IAM users
  * Specify permissions for the entire group
  * No default groups
  * Groups can't be nested
  * A user can belong to multiple groups
  * Permissions are defined using IAM policies


7. IAM roles
  * Used to delegate access to AWS resources
  * Provide temporary access
  * Eliminates the need of static AWS credentials
  * Permission are defined by IAM policies
  * Attached to role, not IAM user or group
  * Roles are granted permissions temporary to applications or AWS services at run time to make requests to other AWS services
  * When creating role
    * Assign `who` can use this role
    * What actions and resources the principal is allowed access to


8. IAM permission
  * check deny first, then allow
  * Explicitly deny?
    * Yes -> deny
    * No -> Check allow
  * Explicitly allow?
    * Yes -> granted
    * No -> deny
  * If neither an explicit deny or explicit allow policy exists, IAM reverts to the default: implicit deny


9. IAM policies
  * Attach to user, group or role
  * Defines actions that may or may not be performed by the entity
  * Single policy can be attached to multiple entities
  * An entity can have multiple policies attached to it
  * Identity based policies - policy apply to user, group or role
    * Managed policies
      * Standalone identity base policies you can attach to multiple user, group and role
    * Inline policies
      * Policy created and managed are embedded directly into a single user, group or role
  * Resources base polices - policy apply to resources such as S3 bucket
    * Json format
    * Control what actions a specified principal can perform on that resource and what conditions
    * Inline policies, no managed resource-based policies


{% include ads3.html %}


### Trusted Advisor
1. Help to reduce cost, increase performance,and improve security by optimizing your AWS environment
  * Cost optimization
    * How to save money by eliminating unused and idle resources or making commitments to reserved capacity
  * Performance
    * Improve performance by checking your service limits.  Make sure advantages of provisioned throughput is taked and monitoring over-utilized instances
  * Security
    * Improve the security of your application by closing gaps, enabling various AWS security features, and examining your permissions
  * Fault tolerance
    * Increase availability and redundancy of your AWS application by take advantage of automatic scaling, health checks, multiple Availability Zones, and backup capabilities
  * Service limits
    * Checks for service usage that is more than 80% of the service limit
  * Color of status check
    * Red: action recommended
    * Yellow: investigation recommended
    * Green: good job

2. Using AWS trusted advisor
  * Free six best practices checks to customers
    * Service limits
    * Security groups - specific ports unrestricted
    * IAM use
    * MFA on root account
    * EBS public snapshots
    * RDS public snapshots


3. Trusted advisor features and functionalities
  * `Notifications` sends weekly email of up to date resource deployment status, free
  * Can use `IAM` to control access to specific checks or categories
  * Support `API` to retrieve and refresh trusted advisor results
  * `Action links` take you directly to web console for further actions
  * `Recent changes` show the recent changes of check status
  * `Exclude items` let you customize what you don't want to see
  * `Five minutes` refresh time default


### CloudTrail
* Enabled by default
* Enables you to simplify governance, compliance, and risk auditing
* Accelerates analysis of operational and security issues by providing visibility into both API and non-API actions in your AWS account


1. Introduction to CloudTrail
  * Web service records API calls across region of your account and deliver logs files to you
  * Everything in AWS is API calls, via CLI, SDK, console or API directly


2. Benefits
  * User and resource activity
    * Who did what at when
  * Simplified compliance audits
    * Automatically recording and storing event logs
  * Always on
  * Security automation
    * Achieved by investigating logs
  * Analysis and troubleshooting
    * Achieved by investigating logs


3. Overview
  * cloudTrail records
    * Who
    * Source IP (from)
    * At what time (when)
    * What action or request        
    * How the request was made
    * Action being performed
    * Which region (location)
    * Response (result)
  * Store `seven days` by default
  * Log can send to S3 for longer retaining time


4. Best practices
  * Make sure service is enable across AWS globally
  * Able to validate the integrity of log files by detecting if they were changed or deleted after they were sent to S3
  * Restrict access to CloudTrail S3 buckets with MFA
  * Integrating with CloudWatch to trigger action when certain events are logged by CloudTrail


### AWS Config
1. Introduction
  * Fully managed service enables you to assess, audit and evaluate the configuration of your AWS resources
  * Provides
    * Resource inventory
    * Configuration history
    * Configuration change notification to enable security and governance
    * Discover existing resources
    * Export complete inventory resource with all configuration details
    * Determine how a resource was configured at any point of time
    * All above enable compliance auditing, security analysis, resource change tracking and troubleshooting


2. Track changes to resources with AWS config
  * Provides AWS resource inventory, configuration history and configuration change notification via SNS
  * Provides continuous details on all configuration changes associated with AWS resources
  * Combines with CloudTrail for full visibility into what contributed to a change
  * Enables compliance auditing, security analysis, resource change tracking and troubleshooting


4. Summary
  * With AWS Config, you can do the following:
    * Evaluate your AWS resource configurations for desired settings
    * Get a snapshot of the current configurations of the supported resources that are associated with your AWS account
    * Retrieve configurations of one or more resources that exist in your account
    * Retrieve historical configurations of one or more resources
    * Receive a notification whenever a resource is created, modified, or deleted
    * View relationships between resources. For example, you might want to find all resources that use a particular security group


### Day One Best Practice Review
1. Day one with AWS
  * Stop using root account
    * Create IAM user accordingly
    * Create IAM group, give full administrator permission and add the IAM user to this group
    * Sign in with IAM user
    * Store root account credentials safely.  Disable and remove root account access key if you have them
  * Require MFA for access
    * MFA for root account and all IAM users
    * MFA can be used on service APIs to control access too
  * Enable CloudTrail
    * Enable cloudTrail to all regions and save logs in S3 bucket
    * Make sure bucket storing CloudTrail logs are access restricted
  * Enable billing report
    * Billing reports provide information about usage of AWS resources and estimate costs for that usage
    * Deliver reports to S3 bucket you specify and updates the report at lease once a day
    * Cost and usage report tracks your usage and provides estimated charges associated with your account, by hour or by day


2. IAM best practices summary
  * Delete root account access key
  * Create individual IAM users
  * Use groups to assign permission to IAM users
  * Grant least privilege
  * configure strong password policy
  * Enable MFA for IAM users
  * Use roles for application run on EC2
  * Delegated by using role instead of by sharing credentials
  * Use policy conditions for extra security
  * Rotate credentials regularly
  * Remove unnecessary users and credentials
  * Monitor activities in your account


### Security and Compliance Programs
1. Compliance approach
  * AWS and customers share control
  *  Responsibility of AWS
    * Provide highly secure and controlled environment
    * Provide wide array of security features
  * Responsibility of the customer
    * Configure IT


2. AWS security information
  * AWS shares security information by
    * Obtaining industry certifications and independent third-party attestations
    * Publishing information about the AWS security and control practices in whitepapers and web site content
    * Providing certificates, reports, and other documentation directly to AWS customers under NDA (as required)


3. AWS assurance programs
  * Certifications/Attestations
    * Compliance certifications and attestations are assessed by a  third-party, independent auditor and result in a certification, audit report, or attestation of compliance
  * Laws, Regulation, and Privacy
    * AWS customers remain responsible for complying with applicable compliance laws and regulations. In some cases, AWS offers functionality (such as security features), enablers, and legal agreements (such as the AWS Data Processing Agreement and Business Associate Addendum) to support customer compliance
  * Alignments/Frameworks
    * Compliance alignments and frameworks include published security or compliance requirements for a specific purpose, such as a specific industry or function.  AWS provides functionality (such as security features) and enablers (including compliance playbooks, mapping documents, and whitepapers) for these types of programs


4. AWS risk and compliance programs
  * Provide information about AWS controls
  * Assist customers in documenting their framework
  * The AWS Risk and Compliance Program is made up of three components:
    * Risk Management
    * Control Environment
    * Information Security


5. AWS risk management
  * Business plan
    * Include rish management
    * Plan re-evaluated at least biannually
  * Responsibilities
    * Identifies risks
    * Implements appropriate measures to address risks
    * Assesses various internal/external risks
  * Information security framework and policies base on
    * Control Objectives for Information and related Technology (COBIT)
    * American Institute of Certified Public Accountants (AICPA)
    * National Institute of Standards and Technology (NIST)
  * AWS take cares of
    * Maintaining the security policy
    * Providing security training to employees
    * Performing application security reviews to assess
      * Data confidentiality, integrity, availability
      * Conformance to IS policy
  * AWS security
    * Scans service endpoints for vulnerabilities
    * Notifies for remediation of vulnerabilities
  * Independent security firms
    * Scans are not replacement for customer scans
    * Customers ca ask to scan cloud infrastructure


6. AWS control environment
  * Includes policies, processes, control activities
  * Secure delivery of AWS service offerings
  * Control environment encompasses
    * People
    * Processes
    * Technology
  * Supports the operating effectiveness of the AWS control
  * Integrate controls identified by industry leading cloud bodies
  * AWS monitors for leading practice ideas to manage control environment


7. Information security
  * Design to protect
    * confidentiality
    * Integrity
    * Availability
  * Publishes security whitepaper


8. Customer compliance requirements
  * Maintain governance over the entire IT control environment
  * Customers should understand
    * Required compliance objectives
    * Validation base risk tolerance
  * Establish control environment
  * Verify effectiveness of control environment
  * Customer compliance basic approach
    * Review
      * Review information available from AWS together with other information to understand as much of the entire IT environment as possible, and then document all compliance requirements
    * Design
      * Design and implement control objectives to meet the enterprise compliance requirements    
    * Identify
      * Identify and document controls owned by outside parties
    * Verify
      * Verify that all control objectives are met and all key controls are designed and operating effectively


### Security Resources
1. Account teams
  * AWS Account Teams provide a first point of contact, guiding you through your deployment and implementation, and pointing you toward the right resources to resolve security issues you may encounter


2. Enterprise support
  * AWS Enterprise Support provides 15-minute response time and is available 24×7 by phone, chat, or email; along with a dedicated Technical Account Manager. This concierge service ensures that customers’ issues are addressed as swiftly as possible


3. Professional services and partner network
  * AWS Professional Services and AWS Partner Network both help customers develop security policies and procedures based on well-proven designs, and help to ensure that customers’ security design meets internal and external compliance requirements
  * The AWS Partner Network has hundreds of certified AWS Consulting Partners worldwide to help customers with their security and compliance needs


4. Advisories and bulletins
  * With AWS Advisories and Bulletins, AWS provides advisories around current vulnerabilities and threats, and enables customers to work with AWS security experts to address concerns like reporting abuse, vulnerabilities, and penetration testing


5. Auditor learning path
  * If you’re in an auditing, compliance, or legal role, check out AWS Auditor Learning Path to get a better understanding of how your internal operations can demonstrate compliance using AWS’ services. You can access Recommended Training, self-paced labs, and auditing resources from the Compliance website
