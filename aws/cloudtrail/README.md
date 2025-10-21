# Deprecation Notice

This module is deprecated. Please use [terraform-aws-cloudtrail](https://github.com/sil-org/terraform-aws-cloudtrail) instead. See [Terraform Registry](https://registry.terraform.io/modules/sil-org/cloudtrail/aws/latest) for more details.

# aws/cloudtrail - CloudTrail
This module is used to set up CloudTrail logging for your AWS account.

## What this does

 - Create S3 bucket for CloudTrail logs
 - Create IAM user with read-only access to CloudTrail S3 bucket
 - (Optional:) Create an Access Key and Secret for that IAM user
 - Enable CloudTrail logging

## Required Inputs

- `s3_bucket_name` - The name for the S3 bucket where your CloudTrail logs will be stored

## Optional Inputs

- `cloudtrail_name` - The name for your Trail in AWS CloudTrail. Default: `"aws-account-cloudtrail"`
- `is_multi_region_trail` - Whether the trail is created in the current region or in all regions. Default: `false`
- `create_access_key` - Whether to create an Access Key/Secret for the created IAM user. Default: `false`

## Outputs

- `s3_access_key_id` - The Access Key ID for the IAM user that has access to the S3 bucket, if requested.
- `s3_access_key_secret` - The Access Key Secret for the IAM user that has access to the S3 bucket, if requested.

## Example Usage

```hcl
module "cloudtrail" {
  source          = "github.com/sil-org/terraform-modules//aws/cloudtrail"
  cloudtrail_name = "${var.cloudtrail_name}"
  s3_bucket_name  = "YOUR-AWS-ACCOUNT-cloudtrail-logs"
}
```
