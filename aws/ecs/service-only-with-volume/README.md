# aws/ecs/service-only - EC2 Container Service Service/Task
This module is used to create an ECS service as well as task definition

## What this does

 - Create task definition
 - Create service

## Required Inputs

 - `cluster_id` - ID for ECS Cluster
 - `service_name` - Name of service, all lowercase, no spaces.
 - `service_env` - Name of environment, used in naming task definition. Ex: `staging`
 - `container_def_json` - JSON for container definition.
 - `desired_count` - Number of tasks to run in service
 - `tg_arn` - Target Group ARN for ALB to register with
 - `lb_container_name` - Container name from `container_def_json` that should be used with target group / alb
 - `lb_container_port` - Container port that should be used with target group / alb
 - `ecsServiceRole_arn` - ARN to IAM ecsServiceRole
 - `volume_name` - Name for volume
 - `volume_host_path` - Path on host EC2 instance to mount volume

### Optional Inputs

 - `task_role_arn` - ARN for role to assign to task definition. Default: `blank`
 - `network_mode` - Networking mode for task. Default: `bridge`
 - `execution_role_arn` - ARN for execution role that allows ECS to make AWS API calls on your behalf, such as to pull container images from ECR when using Fargate or to reference secrets from SSM Parameter Store. Default: `blank`
 - `deployment_maximum_percent` - Upper limit of tasks that can run during a deployment. Default: `200`%
 - `deployment_minimum_healthy_percent` - Lower limit of tasks that must be running during a deployment. Default: `50`%

## Outputs

 - `task_def_arn` - ARN for task definition.
 - `task_def_family` - Family name of task definition.
 - `task_def_revision` - Revision number of task definition.
 - `service_id` - ID/ARN for service
 - `service_name` - Name of service
 - `service_cluster` - Name of ECS cluster service was placed in
 - `service_role` - IAM role for service
 - `service_desired_count` - Desired task count for service

## Usage Example

```hcl
module "ecsservice" {
  source             = "github.com/sil-org/terraform-modules//aws/ecs/service-only"
  cluster_id         = "${module.ecscluster.ecs_cluster_id}"
  service_name       = "${var.app_name}"
  service_env        = "${var.app_env}"
  container_def_json = "${file("task-definition.json")}"
  desired_count      = 2
  tg_arn             = "${data.terraform_remote_state.cluster.alb_default_tg_arn}"
  lb_container_name  = "app"
  lb_container_port  = 80
  ecsServiceRole_arn = "${data.terraform_remote_state.core.ecsServiceRole_arn}"
  volume_name        = "${var.volume_name}"
  volume_host_path   = "${var.volume_host_path}"
  execution_role_arn = "${aws_iam_role.ecs_task_execution_role.arn}"
}
```
