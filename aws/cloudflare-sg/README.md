# Deprecation Notice

This module is deprecated due to changes to the http provider. If the Cloudflare API used by the cloudflare-sg does not return a correct response, the data will be invalid but the Terraform plan will not necessarily fail. This is because the http provider no longer checks the return status but leaves it up to the consumer to check the new status_code attribute.

Instead of using this module, use these resources in your code:

```terraform
resource "aws_security_group" "cloudflare" {
  name        = "cloudflare"
  description = "Allow HTTPS traffic from Cloudflare"
  vpc_id      = module.vpc.id
  tags = {
    Name = "my-app-name-and-environment-cloudflare"
  }
}

resource "aws_security_group_rule" "cloudflare" {
  type              = "ingress"
  from_port         = 443
  to_port           = 443
  protocol          = "tcp"
  security_group_id = aws_security_group.cloudflare.id
  cidr_blocks       = split(",", data.external.cloudflare_ips.result.ipv4_cidrs)
  ipv6_cidr_blocks  = split(",", data.external.cloudflare_ips.result.ipv6_cidrs)
}

data "external" "cloudflare_ips" {
  program = ["${path.module}/cloudflare-ips.sh"]
}
```

cloudflare-ips.sh
```sh
#!/usr/bin/env bash

set -e

curl --silent --fail 'https://api.cloudflare.com/client/v4/ips' | jq '{
  ipv4_cidrs: (.result.ipv4_cidrs | join(",")),
  ipv6_cidrs: (.result.ipv6_cidrs | join(","))
}'
```

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
