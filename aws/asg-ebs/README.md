# aws/asg-ebs - Auto Scaling Group with EBS mount
This module is used to create an auto scaling group launch template and
an auto scaling group that uses the template.  An EBS file system is mounted.

## What this does

 - Create launch template named after `app_name` and `app_env`
 - Create auto scaling group of defined size and distribute instances across `aws_zones`

## Required Inputs

 - `app_name` - Name of application, ex: Doorman, IdP, etc.
 - `app_env` - Name of environment, ex: prod, test, etc.
 - `ami_id` - ID for AMI to be used.
 - `aws_instance` - A map containing keys for `instance_type`, `volume_size`, `instance_count`
 - `aws_region` - Default AWS region
 - `aws_access_key` - Access key to allow access via the AWS CLI
 - `aws_secret_key` - Secret access key to allow access via the AWS CLI
 - `private_subnet_ids` - A list of private subnet ids to identify VPC subnets for placement
 - `default_sg_id` - VPC default security group ID to add instances to
 - `ecs_instance_profile_id` - IAM profile ID for ecsInstanceProfile
 - `ecs_cluster_name` - ECS cluster name for registering instances
 - `ebs_device` - Device name for EBS volume, e.g., "/dev/sdh"
 - `ebs_mount_point` - Full path to directory on which to mount the file system contained in the EBS volume
 - `ebs_vol_id` - EBS volume ID

## Optional Inputs

 - `key_name` - Name of the AWS key pair to allow ssh access, default is ""
 - `root_device_name` - Name of the root device for the EC2 instance. Default: `/dev/xvda`
 - `additional_security_groups` - List of additional security groups (in addition to default vpc security group)
 - `associate_public_ip_address` - true/false - Whether or not to associate public ip addresses with instances. Default: false
 - `additional_user_data` - command to append to the EC2 user\_data, default is ""
 - `ebs_mkfs_label` - Label for filesystem. Default: `Data`
 - `ebs_mkfs_labelflag` - Flag preceding the label name in the mkfs command. Default: `-L`
 - `ebs_mkfs_extraopts` - Extra options to pass to the mkfs command. Default: ""
 - `ebs_fs_type` - Type of filesystem to create. Default: `ext4`
 - `ebs_mountopts` - Mount options to include in /etc/fstab, default is "defaults,noatime"
 - `tags` - Map of tags to be added to all resources, including the network-interface and volume created by the launch template. The `propagate_at_launch` flag will be set true for all tags.
 - `cpu_credits` - Value for the `credit_specification` if you want to override the AWS default for `aws_launch_template`.

## Outputs

 - `launch_template_id` - The launch template ID
 - `auto_scaling_group_id` - ASG ID

## Example Usage

```hcl
module "asg" {
  source = "github.com/sil-org/terraform-modules//aws/asg-ebs"
  app_name = var.app_name
  app_env = var.app_env
  ami_id = module.ecs.ami_id
  aws_instance = var.aws_instance
  aws_region = var.aws_region
  aws_access_key = var.aws_access_key
  aws_secret_key = var.aws_secret_key
  private_subnet_ids = [module.vpc.private_subnet_ids]
  default_sg_id = module.vpc.vpc_default_sg_id
  ecs_instance_profile_id = module.ecs.ecs_instance_profile_id
  ecs_cluster_name = module.ecs.ecs_cluster_name
  additional_user_data = "yum install -y something-interesting"
  ebs_device = "/dev/sdh"
  ebs_mount_point = "/mnt/EBS"
  ebs_vol_id = aws_ebs_volume.bigvol.id
  ebs_mkfs_label = "MyBigFS"
  ebs_mkfs_extraopts = "-m 2 -i 32768"

  tags = {
    foo = bar
  }
}
```
