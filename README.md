## Wordpress-AutoScalable-EFS
This tutorial will walk you through how to setup WordPress High Availability and scable Infra

First of all lets take **ebextensions folder**. Their are bunch of scripts:

**01-dev.config**- This script contains cloud formation stack and creates the Elastic Beanstalk

**02-efs-create.config** - This script helps in the creation of Elastic File System, just you need to provide the Vpc ID and Subnets where you want to create your EFS.

> _Make sure your EC2 instance and EFS are in same VPC_

**03-efs-mount.config** - This script helps in creating the mount directory at your created # Elastic FileSystem

> _Make sure your DNS Hostname is enabled for gettinh your EFS FileSystem ID

> _How to check DNS Hostname enabled--> go to your VPC section in which you are designing your Wordpress--> slect your desired VPC --> under the summary Tab just check that both DNS resolution and DNS hostname are set to "yes", if not then go to action and you will find the edit options_

**04-copywp.config**- This script helps to check if Symlink and wp is already installed if not copy it to EFS mount directory

**05-webmininstall.config**- This script installs webmin for those that like a gui to mange instances.

**06-cronforefs.config**- This script creates a cron job to run the EFS mount script every 5 minutes to check to see if EFS is mounted, if    WordPress is installed in your EFS share, if not it copies it after 5 minutes and then creates a symlink from the /var/app/current Webroot  to the new EFS share web root. It will also check if your symlink is good

> _I have included sample wp.config file also with RDS and Wordpress salt keys values to set them as environment variables, later on modification will be easy, we don't need to ssh every time to changes those values_.

**Note- For getting keys values:**
> https://api.wordpress.org/secret-key/1.1/salt/

# How to Perform Deployment

1. First of all don't forget to update the RDS value and Wordpress salt keys in wp-config file.
2. Provide the VPC ID and Subnets in 02-efs-create.config file
3. Remember to zip the contents of the wordpress not just the top level folder otherwise Beanstalk won't extract the contents  correctly.
4. Deploy zip file to Elastic Beanstalk and wait until the next 5 minute mark for the cron job to do its thing.
5. Log into wordpress via the beanstalk url and create your user name and password
6. Go to route 53 and configure your dns names to point to beanstalk url
7. Remember if your using multisite, use a Route53 A record from your domain names root (mydomain.com > A record with Alias to mydomain.aws-region.beanstalk.com
8. create your certificate in certificate manager (its free so always use SSL) and assign it in the beanstalk ELB in the beanstalk console
9. Log into wordpress using your new URL and change the default site and base url in General setting before enabling multisite
10. Go to tools > network and enable multsite and follow the instructions provided
