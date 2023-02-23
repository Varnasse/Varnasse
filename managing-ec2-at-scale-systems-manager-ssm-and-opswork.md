# Managing EC2 at Scale - Systems Manager (SSM) and OpsWork

### AWS Systems Manager Overview

* Helps you manage your EC2 and On-Premises systems at scale
* Get operational insights about the state of your infrastructure
* Easily detect problems
* Patching automation for enhanced compliance
* Works for both Linux and Windows
* Integrated with CloudWatch metrics / dashboards
* Integrated with AWS Config
* Free service (only pay for what resources it creates)

#### How System Manager Works

* We need to install the SSM agent onto the systems we control
* Installed by default on Amazon Linux 2 AMI
* If an instance cannot be controlled with SSM, it is probably an issue with the SSM agent
* Make sure the EC2 instances have proper IAM role to allow SSM actions

### AWS Tags

* You can add text key-value pairs called tags to many AWS resources
* Free naming, common tags are Name, Environment, Team...

#### Resource Groups

* Create, view or manage logical group of resources thanks to tags
* Allows creation of logical groups of resources such as:
  * Applications
  * Different layers of an application stack
  * Production versus development environments
* Regional Service
* Works with EC2, S3, DynamoDB, Lambda, etc..

### SSM - Documents

* Documents can be in JSON or YAML
* You define the parameters and actions
* Many documents already exist in AWS

#### SSM - Run Command

* Execute a document (a script) or just run a command
* Run command across multiple instances (using resource groups)
* Rate control / Error control
* Integrated with IAM and CloudTrail
* No need for SSH
* Command output can be shown in the Console, or sent to S3 buckets or CloudWatch Logs
* Send notifications to SNS about command status (in progress, success, failed)
* Can be invoked using EventBridge

#### SSM - Automation

* Simplifies common maintenance and deployment tasks of EC2 instances and other AWS resources
* Example: restart instances, create an AMI or EBS snapshot
* Automation Runbook:
  * SSM Documents of type automation
  * Defines actions performed on your EC2 instances or AWS resources
  * Pre-defined runbooks (AWS) or create custom runbooks
* Can be triggered:
  * Manually using AWS Console, AWS CLI or SDK
  * By Amazon EventBridge
  * On a schedule using Maintenance Windows
  * By AWS Config for rules remediations

### SSN Parameter Store

* Secure storage for configuration and secrets
* Optional seamless encryption using KMS
* Serverless, scalable, durable, easy SDK
* Version tracking of configurations / secrets
* Security through IAM
* Notifications with Amazon EventBridge

#### SSM Parameter Store Hierarchy

* /my-department
  * my-app/
    * dev/
      * db-url
      * db-password
    * prod/
      * db-url
      * db-password
  * other-app
* other-department/

#### Parameter Policies

* Allow a time-to-live to a parameter (Expiration date) to force updating or deleting sensitive data
* Can assign multiple policies at a time

### SSM Inventory & State Manager

* Collect metadata from your managed instances
* Metadata includes installed software, OS drivers, configurations, installed updates
* View data in AWS console or store in S3 and query and analyse using Athena or QuickSight
* Specify metadata collection interval (minutes, hours, days)
* Query data from multiple AWS accounts and regions
* Create custom inventory for your custom metadata (e.g. rack location of each managed instance)

#### SSM - State Manager

* Automate the process of keeping your managed instances (EC2/On-premises) in a state that you define
* Use cases: bootstrap instances with software, patch OS/software updates on a schedule
* State Manager Association:
  * Defines the state that you want to maintain to your managed instances
  * Example: port 22 must be closed, antivirus must be installed
  * Specify a schedule when this configuration is applied
* Uses SSM Documents to create an Association (e.g. SSM Document to configure CloudWatch agent)

### SSM - Patch Manager

* Automates the process of patching managed instances
* OS updates, application updates, security updates
* Supports both EC2 instances and on-premises servers
* Patch on-demand or on a schedule using Maintenance Windows
* Scan instances and generate patch compliance report (missing patches)

#### Patch Baseline

* Defines which patches should and shouldn't be installed&#x20;
* Ability to create custom patch baselines
* Patches can be auto-approved within days of their release
* By default, install only critical patches and patches related to security

#### Patch Group

* Associate a set of instances with a specific Patch Baseline
* Example: create Patch Groups for different environments
* Instances should be defined with the tag key Patch Group
* An instance can only be in one Patch Group
* Patch Group can be registered with only one Patch Baseline

### SSM - Session Manager

* Allows you to start a secure shell on your EC2 and on-premises servers
* Access through AWS Console, AWS CLI, or Session Manager SDK
* Does not need SSH access, bastion hosts or SSH keys]
* Log connections to your instancces and executed commands
* Session log data can be sent to S3 or CloudWatch logs
* CloudTrail can intercept StartSession event
* IAM Permissions
  * Control which users/groups can access Session manager and which instances
  * Use tags to restrict access to only specific EC2 instances
  * Access SSM + write to S3 + write to CloudWatch

### AWS OpsWorks Overview

* Chef & Puppet help you perform server configuration automatically or repetitive actions
* They work great with EC2 and on-premises VMs
* AWS OpsWorks = Managed Chef and Puppet
* It's an alternative to AWS SSM



