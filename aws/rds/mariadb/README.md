# Deprecation Notice

This module is deprecated. In keeping with the best practice recommended [here](https://developer.hashicorp.com/terraform/language/modules/develop/composition), simply include the code in this module directly in your module.

# aws/rds/mariadb - Relational Database Service - MariaDB
This module is used to create an RDS instance.

## What this does

 - Create RDS instance named after `db_name`

## Required Inputs

 - `app_name` - Name of application, ex: Doorman, IdP, etc.
 - `app_env` - Name of environment, ex: prod, test, etc.
 - `db_name` - Name of database to be created by default
 - `db_root_user` - Root database username
 - `db_root_pass` - Root database account password
 - `subnet_group_name` - Name of DB subnet group to place instance in
 - `security_groups` - List of security group ids to assign instance to

### Optional Inputs

 - `apply_immediately` - Default: `false` - description = Apply database modifications immediately, rather than during the next maintenance window. CAUTION: may cause downtime!
 - `availability_zone` - Default: `""` - Availability zone name to place instance in
 - `auto_minor_version_upgrade` - Default: `false` - description = Indicates that minor engine upgrades will be applied automatically during the maintenance window
 - `allow_major_version_upgrade` -Default: `false` - description = Indicates that major version upgrades are allowed. Change this to true when upgrading major versions
 - `ca_cert_identifier` - Default: `null` - The identifier of the CA certificate for the DB instance.
 - `copy_tags_to_snapshot` - Default: `true`
 - `engine` - Default: `mariadb`
 - `engine_version` - Default: `<empty>`
 - `allocated_storage` - Default: `8`G
 - `storage_type` - Default: `gp2`
 - `storage_encrypted` - Default: `false`
 - `kms_key_id` - Default: `""`
 - `instance_class` - Default: `db.t2.micro`
 - `backup_retention_period` - Default: `14` days
 - `multi_az` - Default: `false`
 - `publicly_accessible` - Default: `false`
 - `skip_final_snapshot` - Default: `true`
 - `parameter_group_name` - Default: `""`
 - `deletion_protection` - Default: `false`
 - `tags` - Default `{}`
 - `replicate_source_db` - Default `""`

## Outputs

 - `id` - RDS instance ID
 - `arn` - ARN of RDS instance ID
 - `address` - Address for RDS instance
 - `port` - Port for RDS instance
 - `endpoint` - Full address+port of RDS instance

## Example Usage

```hcl
module "rds" {
  source = "github.com/sil-org/terraform-modules//aws/rds/mariadb"
  app_name = "${var.app_name}"
  app_env = "${var.app_env}"
  db_name = "${var.db_name}"
  db_root_user = "${var.db_root_user}"
  db_root_pass = "${var.db_root_pass}"
  subnet_group_name = "${module.vpc.db_subnet_group_name}"
  availability_zone = "${var.aws_zones[0]}"
  security_groups = ["${module.vpc.vpc_default_sg_id}"]

  tags = {
    foo = "bar"
  }
}
```
