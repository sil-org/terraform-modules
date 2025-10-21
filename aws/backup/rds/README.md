# Deprecation Notice

This module is deprecated. Please use [terraform-aws-backup](https://github.com/sil-org/terraform-aws-backup) instead. See [Terraform Registry](https://registry.terraform.io/modules/sil-org/backup/aws/latest) for more details. 

# aws/backup/rds - AWSBackup of databases
This module is used to create scheduled backups of AWS databases.


## Required Inputs

- `app_name` - Name of application, ex: Doorman, IdP, etc.
- `app_env` - Name of environment, ex: prod, test, etc.
- `source_arns` - List of ARN's of the databases to backup

### Optional Inputs

- `backup_cron_schedule` - Default: "11 1 * * ? *"
- `notification_events` - Default: ["BACKUP_JOB_STARTED", "BACKUP_JOB_COMPLETED", "BACKUP_JOB_FAILED", "RESTORE_JOB_COMPLETED"]
- `aws_access_key` - Not needed and not used
- `aws_secret_key` - Not needed and not used


## Outputs

- `bkup_key_id` - The backup key ID
- `bkup_key_arn` - The backup key ARN
- `bkup_vault_arn` - The backup vault ARN
- `bkup_cron_scheduled` - The cron schedule for making backups
- `sns_notification_events` - The events that trigger SNS notifications


## Example Usage

*vars.tf* (in addition to `app_name`, `app_env`, ...)
```hcl
variable "enable_db_backup" {
  type    = bool
  default = false
}
```


*main.tf*
```hcl
module "backup_rds" {
  count = var.enable_db_backup ? 1 : 0
  source = "github.com/sil-org/terraform-modules//aws/backup/rds"
  app_name = var.app_name
  app_env = var.app_env
  aws_access_key = var.aws_access_key
  aws_secret_key = var.aws_secret_key
  source_arns = ["arn:aws:rds:us-east-1:123456789012:db:my-db"]
  backup_cron_schedule = "11 1 * * ? *"
  notification_events = ["BACKUP_JOB_STARTED", "BACKUP_JOB_COMPLETED", "BACKUP_JOB_FAILED", "RESTORE_JOB_COMPLETED"]
}
```

*outputs.tf*
```hcl
output "bkup_key_id" {
  value = var.enable_db_backup ? module.backup_rds[0].bkup_key_id : "backup disabled"
}
```
