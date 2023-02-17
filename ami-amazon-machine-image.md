# AMI - Amazon Machine Image

### AIM Overview

* Amazon Machine Images are a customisation of an EC2 instance
  * You can add your own software, configuration, operating system, monitoring..
  * Faster boot / configuration time because all your software if pre-packaged
* AMI are built for a specific region (and can be copied across regions)
* You can launch EC2 instances from:
  * A public AMI: AWS provided
  * Your own AMI: you make and maintain them yourself
  * An AWS Marketplace AMI: an AMI made and sold by someone else

#### AMI Process (from an EC2 instance)

* Start an EC2 instance and customise it
* Stop the instance (for data integrity)
* Build an AMI - this will also create EBS snapshots
* Launch instances from other AMIs

### AMI No-Reboot Option

* Enables you to create an AMI without shutting down your instance
* By default, it's not selected (AWS will shut down the instance before creating an AMI to maintain the file system integrity)

#### AWS Backup Plans to Create AMI

* AWS Backup does not reboot the instances while taking EBS snapshots (no-reboot behaviour)
  * This won't help you to create an AMI that guarantees file system integrity since you need to reboot the instance
  * To maintain integrity, you need to provide the reboot parameter while taking images (EventBridge + Lambda + CreateImage API with reboot)

### Cross-Account AMI Sharing

* You can share an AMI with another AWS account
* Sharing an AMI does not affect the ownership of the AMI
* You can only share AMIs that have unencrypted volumes and volumes that are encrypted with a customer-managed key
* If you share an AMI with encrypted volumes, you must also share any customer managed keys used to encrypt them.

### Cross-Account AMI Copy

* If you copy an AMI that has been shared with your account, you are the ownder of the target AMI in your account
* The owner of the source AMI must grant you read permissions for the storage that backs the AMI (EBS Snapshot)
* If the shared AMI has encrypted snapshots, the owner must share the key(s) with you, as well.

### EC2 Image Builder

* Used to automate the creation of VMs or container images
* Automate the creation, maintain, validate and test EC2 AMIs
* EC2 Image Builder -> Creates a 'builder EC2 instance' -> New AMI is created -> Image builder then tests the instance -> AMI is then distributed (can be multiple regions)
* EC2 Image Builder can be run on a schedule
* Free service (only pay for the underlying resources)&#x20;

### AMI in Production

* You can force users to only launch EC2 instances from pre-approved AMIs (AMIs tagged with specific tags) using IAM policies
* Combine with AWS Config to find non-compliant EC2 instances (instances launched with non-approved AMIs)
*
