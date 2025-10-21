# aws/ecs/elasticache/redis - Create a Elasticache cluster of Redis servers
This module is used to create an Elasticache cluster of Redis servers

## What this does

 - Create Elasticache subnet group spanning all provided `var.subnet_ids`.
 - Create Elasticache cluster of Redis servers.

## Required Inputs

 - `cluster_id` - ID/Name for Elasticache Cluster. Max 20 characters.
 - `security_group_ids` - List of security group IDs to place cluster in
 - `subnet_group_name` - Name of subnet group to create and place cluster in.
 - `subnet_ids` - List of subnet ids for subnet group.
 - `availability_zones` - List of availability zones to place cluster in.
 - `app_name` - Application name to be tagged to cluster as `app_name`.
 - `app_env` - Application environment to be tagged to cluster as `app_env`.

### Optional Inputs

 - `node_type` - Instance node type. Default: `cache.t2.micro`
 - `port` - Redis port. Default: `6379`
 - `parameter_group_name` - Name of Redis parameter group to use. Default: `default.redisd1.4`
 - `engine_version` - Redis engine version. Default: `6.x`'

## Outputs

 - `cache_nodes` - List of cache nodes.
 - `configuration_endpoint` - Redis configuration endpoint.
 - `cluster_address` - DNS name for cache clusters without port.

## Usage Example

```hcl
module "redis" {
  source = "github.com/sil-org/terraform-modules//aws/elasticache/redis"
  cluster_id = "doorman-cache"
  security_group_ids = [module.vpc.vpc_default_sg_id]
  subnet_group_name = "doorman-cache-subnet"
  subnet_ids = module.vpc.private_subnet_ids
  availability_zones = module.vpc.aws_zones
  app_name = "doorman"
  app_env = "staging"
}
```
