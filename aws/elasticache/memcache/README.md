# aws/ecs/elasticache/memcache - Create a Elasticache cluster of Memcache servers
This module is used to create an Elasticache cluster of memcache servers

## What this does

 - Create Elasticache subnet group spanning all provided `var.subnet_ids`.
 - Create Elasticache cluster of memcache servers.

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
 - `port` - Memcache port. Default: `11211`
 - `num_cache_nodes` - Number of cache nodes. Default: `2`
 - `parameter_group_name` - Name of Memcache parameter group to use. Default: `default.memcached1.4`
 - `az_mode` - Whether or not to use multiple zones. Default: `cross-az`. For single use `single-az`.

## Outputs

 - `cache_nodes` - List of cache nodes.
 - `configuration_endpoint` - Memcache configuration endpoint.
 - `cluster_address` - DNS name for cache clusters without port.

## Usage Example

```hcl
module "memcache" {
  source = "github.com/sil-org/terraform-modules//aws/elasticache/memcache"
  cluster_id = "doorman-cache"
  security_group_ids = ["${module.vpc.vpc_default_sg_id}"]
  subnet_group_name = "doorman-cache-subnet"
  subnet_ids = ["${module.vpc.private_subnet_ids}"]
  availability_zones = ["${module.vpc.aws_zones}"]
  app_name = "doorman"
  app_env = "staging"
}
```
