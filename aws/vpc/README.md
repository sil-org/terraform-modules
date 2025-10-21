# Deprecation Notice

This module is deprecated. Please use [terraform-aws-vpc](https://github.com/sil-org/terraform-aws-vpc) instead. See [Terraform Registry](https://registry.terraform.io/modules/sil-org/vpc/aws/latest) for more details.

# aws/vpc - Virtual Private Cloud
This module is used to create a VPC along with the necessary configuration to
be useful.

## What this does

- Create VPC named after `app_name` and `app_env`
- Create public and private subnets for each `aws_zones` specified
- Provision a Internet Gateway and configure public subnets to route through it
- Provision a NAT Gateway (or use an existing Transit Gateway) and configure private subnets to route through it
- Create a DB subnet group including all private subnets
- Optionally allocate IPv6 CIDR blocks, egress-only internet gateway, and default IPv6 routes

## Required Inputs

- `app_name` - Name of application, ex: Cover, IdP, etc.
- `app_env` - Name of environment, ex: prod, test, etc.

## Optional Inputs

- `aws_zones` - A list of zones to create subnets (max 8) - default: `["us-east-1c", "us-east-1d", "us-east-1e"]`
- `enable_dns_hostnames` - default `false`
- `create_nat_gateway` - default `true`
- `private_subnet_cidr_blocks` - default: `["10.0.11.0/24", "10.0.22.0/24", "10.0.33.0/24", "10.0.44.0/24"]`
- `public_subnet_cidr_blocks` - default: `["10.0.10.0/24", "10.0.20.0/24", "10.0.30.0/24", "10.0.40.0/24"]`
- `vpc_cidr_block` - default: `"10.0.0.0/16"`
- `use_transit_gateway` - default `false`
- `transit_gateway_id` - required if `use_transit_gateway` is true
- `transit_gateway_default_route_table_association` - default `true`
- `transit_gateway_default_route_table_propagation` - default `true`
- `enable_ipv6` - default `false` 

## Outputs

- `vpc_default_sg_id` - The VPC default security group ID
- `public_subnet_ids` - A list of the public subnet IDs
- `public_subnet_cidr_blocks` - A list of public subnet CIDR blocks, ex: `["10.0.10.0/24","10.0.20.0/24"]`
- `private_subnet_ids` - A list of the private subnet IDs
- `private_subnet_cidr_blocks` - A list of private subnet CIDR blocks, ex: `["10.0.11.0/24","10.0.22.0/24"]`
- `db_subnet_group_name` - The name of the DB subnet group
- `ipv6_association_id` - The association ID for the IPv6 CIDR block.
- `ipv6_cidr_block` - The IPv6 CIDR block assigned to the VPC.

## Example Usage

```hcl
module "vpc" {
  source = "github.com/sil-org/terraform-modules//aws/vpc"
  app_name = var.app_name
  app_env = var.app_env
  aws_zones = var.aws_zones
}
```
