# CloudFormation for SysOps

### Overview

CloudFormation is a declarative way of outlining your AWS infrastructure, for any resource

Benefits of AWS CloudFormation

* Infrastructure as code
  * No resources are manually created, which is excellent for control
  * The code can be version controlled for example using git
  * Changes to the infrastructure are reviewed through code
* Cost
  * Each resource within the stack is staged with an identifier so you can easily see how much a stack costs you
* Productivity
  * Ability to destroy and re-create an infrastructure on the cloud&#x20;
  * Automated generation of diagram for your templates
  * Declarative programming
* Separation of concern: create many stacks for many apps, and many layers
  * VPC stacks
  * Network stacks
  * App stacks
* Don't re-invent the wheel
  * Leverage existing templates

### How does CloudFormation work?

* Templates have to be uploaded in S3 and then referenced in CloudFormation
* To update a template, we can't edit previous ones. We have to re-upload a new version of the template.
* Stacks are identified by name
* If you delete a stack, every single artifact will be destroyed with it

### CloudFormation: Building Blocks

1. Resources: Your AWS resources declared in the template (mandatory)
2. Parameters: The dynamic inputs for your template
3. Mappings: The static variables for your template
4. Outputs: References to what has been created
5. Conditionals: List of conditions to perform resource creation
6. Metadata

### Parameters

* Ask yourself this:
  * Is this CloudFormation resource configuration likely to change in the future?
  * If so, make it a parameter

#### How to reference a parameter

* The Fn::Ref function can be leveraged to reference parameters
* Parameters can be used anywhere in a template
* The shorthand in the template is !REF

### Mappings

* Fixed variables wihtin your CloudFormation template
* They are handy to differentiate between different environments (dev vs prod), regions, AMI types, etc
* All the values are hardcoded within the template

#### When would you use mappings vs parameters

* Mappings are great when you know in advance all the values that can be taken and that they can be deduced from variables,  such as:
  * Region
  * AZ
  * AWS account
  * Environment
* They allow safer control over the template
* We use the FN::FindInMap to return a named value from a specific key
* !FindInMap \[ MapName, TopLevelKey, SecondLevelKey ]

### What are outputs?

* The output section declares _optional_ values that we can import into other stacks (if you export  them first)
* You can also view the outputs in the AWS console or in the AWS CLI
* It's the best way to perform some collaboration cross stack

#### Cross Stack Reference

* We then create a second template that leverages that security group
* For this, we use the Fn::ImportValue function
* You cannot delete the underlying stack until all the references are deleted too

### Conditions

* Used to control the creation of resources or outputs based on conditions
* Example
  * Conditions:
    * CreateProdResources: !Equals \[ !Ref EnvType, prod ]

### Intrinsic Functions

* Fn::Ref -> can be leveraged to reference
  * Parameters
  * Resources
  * !Ref
* Fn:GetAtt returns a value for a specified attribute of this type
* We use the FN::FindInMap to return a named value from a specific key
* !FindInMap \[ MapName, TopLevelKey, SecondLevelKey ]
* Fn::ImportValue allows us to import values that are exported in other templates
* Fn::Join allows you to join values with a delimiter
* Fn::Sub allows you to substitute variables from a text

### User Data in EC2 for CloudFormation

* We can have user data at EC2 instance launch through the console and we can also include it in CloudFormation
* The important thing is to pass the entire script through the function FN::Base64
* Good to know: user data script log is in /var/log/cloud-init-output.log

### cfn-init

* AWS::CloudFormation::Init must be in the metadata of a resource
* With the cfn-init script, it helps make complex EC2 configurations readable
* The EC2 instance will query the CloudFormation service to get init data
* Logs to to /var/log/cfn-init.log

### cfn-signal & wait conditions

* We still don't know how to tell CloudFormation that the EC2 instance was properly configured after a cfn-init
* For this, we can use the cfn-signal script
  * We run cfn-signal right after cfn-init
  * Tell CloudFormation service to keep going or fail
* We need to define a WaitCondition:
  * Block the template until it receives a signal from cfn-signal
  * We attach a CreationPolicy&#x20;

### CloudFormation Rollbacks

* Stack Creation Fails:
  * Default: everything rolls back (gets deleted)
  * Option to disable rollback and troubleshoot what happened
* Stack update fails:
  * The stack automatically rolls back to the previous known working state
  * Ability to see in the log what happened and error messages
*
