# Deprecation Notice

This module is deprecated. Please use [terraform-aws-alb](https://github.com/sil-org/terraform-aws-alb) instead. See [Terraform Registry](https://registry.terraform.io/modules/sil-org/alb/aws/latest) for more details.

# aws/alb - Application Load Balancer
This module is used to create an application load balancer along with security
groups for traffic and a default target group.

## What this does

 - Create ALB named after `app_name` and `app_env`
 - Create ALB target group
 - Create HTTPS listener for ALB / Target Group

## Required Inputs

 - `app_name` - Name of application, ex: Doorman, IdP, etc.
 - `app_env` - Name of environment, ex: prod, test, etc.
 - `vpc_id` - ID of VPC for target group.
 - `security_groups` - List of security groups to apply to ALB.
 - `subnets` - A list of public subnet ids for ALB placement
 - `certificate_arn` - ARN to SSL certificate to use with HTTPS listener

### Optional Inputs
 - `port` - Target group listening port. Default: `80`
 - `protocol` - Target group listening protocol. Default: `http`
 - `health_check_interval` - Default: `30`
 - `health_check_path` - Default: `/`
 - `health_check_port` - Default: `traffic-port`
 - `health_check_protocol` - Default: `HTTP`
 - `health_check_timeout` - Default: `5`
 - `healthy_threshold` - Default: `5`
 - `unhealthy_threshold` - Default: `2`
 - `health_check_status_codes` - Default: `200`, separate multiple values with comma, ex: `200,204`
 - `idle_timeout` - Default: `60`
 - `load_balancer_type` - Options: `application` or `network`. Default: `application`
 - `enable_ipv6` - Set to `true` to enable IPv6. Changes the ALB `ip_address_type` to `"dualstack"`. Default: `false`

## Outputs

 - `id` - ID for ALB
 - `arn` - ARN for ALB
 - `dns_name` - DNS hostname for ALB
 - `default_tg_id` - ID for Target Group
 - `default_tg_arn` - ARN for Target Group

## Example Usage

```hcl
module "asg" {
  source = "github.com/sil-org/terraform-modules//aws/asg"
  app_name = "${var.app_name}"
  app_env = "${var.app_env}"
  vpc_id = "${module.vpc.id}"
  security_groups = ["${module.vpc.vpc_default_sg_id}","${module.cloudflare-sg.id}"]
  subnets = ["${module.vpc.public_subnet_ids}"]
  certificate_arn = "${data.aws_acm_certificate.name.arn}"
}
```
