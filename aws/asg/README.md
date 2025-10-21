# aws/asg - Auto Scaling Group
This module is used to create an auto scaling group launch template and
an auto scaling group that uses the template.

## What this does

 - Create launch template named after `app_name` and `app_env`
 - Create auto scaling group of defined size and distribute instances across `aws_zones`

## Required Inputs

 - `app_name` - Name of application, ex: Doorman, IdP, etc.
 - `app_env` - Name of environment, ex: prod, test, etc.
 - `ami_id` - ID for AMI to be used.
 - `aws_instance` - A map containing keys for `instance_type`, `volume_size`, `instance_count`
 - `private_subnet_ids` - A list of private subnet ids to identify VPC subnets for placement
 - `default_sg_id` - VPC default security group ID to add instances to
 - `ecs_instance_profile_id` - IAM profile ID for ecsInstanceProfile
 - `ecs_cluster_name` - ECS cluster name for registering instances

## Optional Inputs

 - `key_name` - Name of the AWS key pair to allow ssh access, default is ""
 - `root_device_name` - Name of the root device for the EC2 instance. Default: `/dev/xvda`
 - `additional_security_groups` - List of additional security groups (in addition to default vpc security group)
 - `associate_public_ip_address` - true/false - Whether or not to associate public ip addresses with instances. Default: false
 - `additional_user_data` - command to append to the EC2 user\_data, default is ""
 - `tags` - Map of tags to be added to all resources, including the network-interface and volume created by the launch template. The `propagate_at_launch` flag will be set true for all tags.
 - `cpu_credits` - Value for the `credit_specification` if you want to override the AWS default for `aws_launch_template`.
 - `enable_ipv6` - Set to true to add an IPv6 IP address to ASG-created instances. Default: `false`

## Outputs

 - `launch_template_id` - The launch template ID
 - `auto_scaling_group_id` - ASG ID

## Example Usage

```hcl
module "asg" {
  source = "github.com/sil-org/terraform-modules//aws/asg"
  app_name = "${var.app_name}"
  app_env = "${var.app_env}"
  aws_instance = "${var.aws_instance}"
  private_subnet_ids = ["${module.vpc.private_subnet_ids}"]
  default_sg_id = "${module.vpc.vpc_default_sg_id}"
  ecs_instance_profile_id = "${module.ecs.ecs_instance_profile_id}"
  ecs_cluster_name = "${module.ecs.ecs_cluster_name}"
  ami_id = "${module.ecs.ami_id}"
  additional_user_data = "yum install -y something-interesting"

  tags = {
    foo = bar
  }
}
```
