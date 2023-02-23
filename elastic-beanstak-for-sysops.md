# Elastic Beanstak for SysOps

### Overview

* Elastic Beanstalk is a developer centric view of deploying an application on AWS
* Managed service
  * Automatically handles capacity provisioning, load balancing, scaling, application health monitoring, instance configuration
  * The application code is the responsibility of the developer
* We still have full control over the configuration
* Beanstalk is free but you pay for the underlying services launched

### Components

* Application: Collection of Elastic Beanstalk components (environments, versions, configurations...)
* Application version: an iteration of your application code
* Environment
  * Collection of AWS resources running an application version (only one version at a time)
  * Tiers: web server environment tier (traditional architecture: load balancer -> auto scaling group -> EC2 instances) & worker environment tier (no clients accessing the EC2 instances. It uses an SQS queue to auto scaling group and the EC2 instances act as workers).
*
