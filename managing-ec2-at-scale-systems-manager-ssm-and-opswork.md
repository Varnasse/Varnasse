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
*

