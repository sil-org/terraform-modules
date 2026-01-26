# Deprecation Notice

This module is deprecated due to changes to the http provider. If the Cloudflare API used by the cloudflare-sg does not return a correct response, the data will be invalid but the Terraform plan will not necessarily fail. This is because the http provider no longer checks the return status but leaves it up to the consumer to check the new status_code attribute.

Instead of using this module, use this data source in your code:

```terraform
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

# cloudflare/ips - Cloudflare IP Addresses
This module is used to pull the current list of IP ranges for Cloudflare for IPv4 and IPv6

## What this does

 - Provide outputs for `ipv4_cidrs` and `ipv6_cidrs`

## Required Inputs

 ~none~

## Outputs

 - `ipv4_cidrs` - List of IPv4 IP ranges
 - `ipv6_cidrs` - List of IPv6 IP ranges

## Example Usage

```hcl
module "cf_ips" {
  source = "github.com/sil-org/terraform-modules//cloudflare/ips"
}

resource "aws_security_group" "cloudflare_https" {
  name        = "cloudflare-https"
  description = "Allow HTTPS traffic from Cloudflare"
  vpc_id      = "${var.vpc_id}"
}

resource "aws_security_group_rule" "cloudflare_ipv4" {
  type              = "ingress"
  from_port         = 443
  to_port           = 443
  protocol          = "tcp"
  security_group_id = "${aws_security_group.cloudflare_https.id}"
  cidr_blocks       = ["${module.cf_ips.ipv4_cidrs}"]
}
```
