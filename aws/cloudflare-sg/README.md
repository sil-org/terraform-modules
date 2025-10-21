# aws/cloudflare-sg - Cloudflare Security Group
This module is used to create a security group to allow HTTPS/443 traffic from
Cloudflare IP addresses only.

## What this does

 - Create security group allowing HTTPS traffic on port 443 from Cloudflare IPs

## Required Inputs

 - `vpc_id` - ID of VPC to place security group

## Outputs

 - `id` - ID of security group created
 - `name` - Name of security group created

## Example Usage

```hcl
module "cloudflare-sg" {
  source = "github.com/sil-org/terraform-modules//aws/cloudflare-sg"
  vpc_id = "${var.vpc_id}"
}
```
