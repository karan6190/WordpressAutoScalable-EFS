# WordpressAutoScalable-EFS
This tutorial will walk you through how to setup WordPress High Availability and scable Infra

First of all lets take ebextension folder. Their are bunch of scripts:
**01-dev.config**- This script contains cloud formation stack and creates the Elastic Beanstalk
**02-efs-create.config** - This script helps in the creation of Elastic File System, just you need to provide the Vpc ID and Subnets where you want to create your EFS.

> _Make sure your EC2 instance and EFS are in same VPC_

**03efs-mount.config** - This script helps in creating the mount directory at your created # Elastic FileSystem

> _Make sure your DNS Hostname is enabled for gettinh your EFS FileSystem ID

> _How to check DNS Hostname enabled--> go to your VPC section in which you are designing your Wordpress--> slect your desired VPC --> under the summary Tab just check that both DNS resolution and DNS hostname are set to "yes", if not then go to action and you will find the edit options_

**04copywp.config**- This script helps to check if Symlink and wp is already installed if not copy it to EFS mount directory
**05webmininstall.config**- This script installs webmin for those that like a gui to mange instances.
**06cronforefs.config**- This script creates a cron job to run the EFS mount script every 5 minutes to check to see if EFS is mounted, if    WordPress is installed in your EFS share, if not it copies it after 5 minutes and then creates a symlink from the /var/app/current Webroot  to the new EFS share web root. It will also check if your symlink is good

> _I have included sample wp.config file also with RDS and Keys values to set them as environment variables, later on modification will be easy, we don't need to ssh every time to changes those values_.

**Note- For getting keys values:**
> https://api.wordpress.org/secret-key/1.1/salt/
