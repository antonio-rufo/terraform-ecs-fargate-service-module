# AWS ECS Fargate Terraform Module #

This Terraform module deploys an AWS ECS Fargate service.

## Requirements

| Name      | Version |
| --------- | ------- |
| terraform | >= 0.12 |

## Providers

| Name | Version |
| ---- | ------- |
| aws  | n/a     |

## Inputs

| Name                                  | Description                                                         | Type           | Default | Required |
| --------------------------------------| ------------------------------------------------------------------- | -------------- | ------- | :------: |
| name\_prefix                          | Name prefix for resources on AWS                                    | `list`         | n/a     |   yes    |
| vpc\_id                               | ID of the VPC                                                       | `any`          | n/a     |   yes    |
| ecs\_cluster\_arn                     | ARN of an ECS cluster                                               | `string`       | n/a     |   yes    |
| deployment\_maximum\_percent          | (Optional) The upper limit (as a percentage of the service's desiredCount) of the number of running tasks that can be running in a service during a deployment. | `number` | `200` | no |
| deployment\_minimum\_healthy\_percent | (Optional) The lower limit (as a percentage of the service's desiredCount) of the number of running tasks that must remain running and healthy in a service during a deployment. | `number` | `100` | no |
| desired\_count                        | (Optional) The number of instances of the task definition to place and keep running. Defaults to 0. | `number` | `1` | no |
| enable\_ecs\_managed\_tags            | (Optional) Specifies whether to enable Amazon ECS managed tags for the tasks within the service. | `bool` | `false` | no |
| health\_check\_grace\_period\_seconds | (Optional) Seconds to ignore failing load balancer health checks on newly instantiated tasks to prevent premature shutdown, up to 2147483647. Only valid for services configured to use load balancers. | `number` | `0` | no |
| ordered\_placement\_strategy          | (Optional) Service level strategy rules that are taken into consideration during task placement. List from top to bottom in order of precedence. The maximum number of ordered_placement_strategy blocks is 5. This is a list of maps where each map should contain \"id\" and \"field\" | `list(any)` | `[]` | no |
| placement\_constraints                | (Optional) rules that are taken into consideration during task placement. Maximum number of placement_constraints is 10. This is a list of maps, where each map should contain \"type\" and \"expression\" | `list(any)` | `[]` | no |
| platform\_version                     | (Optional) The platform version on which to run your service. Defaults to 1.4.0. More information about Fargate platform versions can be found in the AWS ECS User Guide. | `string` | `1.4.0` | no |
| propagate\_tags                       | (Optional) Specifies whether to propagate the tags from the task definition or the service to the tasks. The valid values are SERVICE and TASK_DEFINITION. Default to SERVICE | `string` | `SERVICE` | no |
| service\_registries                   | (Optional) The service discovery registries for the service. The maximum number of service_registries blocks is 1. This is a map that should contain the following fields \"registry_arn\", \"port\", \"container_port\" and \"container_name\" | `map(any)` | `{}` | no |
| task\_definition\_arn                 | (Required) The full ARN of the task definition that you want to run in your service. | `string` | n/a | yes |
| force\_new\_deployment                | (Optional) Enable to force a new task deployment of the service. This can be used to update tasks to use a newer Docker image with same image/tag combination (e.g. myimage:latest), roll Fargate tasks onto a newer platform version, or immediately deploy ordered_placement_strategy and placement_constraints updates. | `bool` | `false` | no |
| public\_subnets                       | The public subnets associated with the task or service.           | `list(any)`          | n/a     |   yes   |
| private\_subnets                      | The private subnets associated with the task or service.            | `list(any)`          | n/a     |   yes   |
| security\_groups                      | (Optional) The security groups associated with the task or service. If you do not specify a security group, the default security group for the VPC is used.               | `list(any)`          | `[]`     |   no   |
| assign\_public\_ip                    | (Optional) Assign a public IP address to the ENI (Fargate launch type only). If true service will be associated with public subnets. Default false.            | `bool`          | `false`     |   no   |
| container\_name                       | Name of the running container | `string` | n/a | yes |
| enable\_autoscaling                   | (Optional) If true, autoscaling alarms will be created. | `bool` | `true` | no |
| ecs\_cluster\_name                    | (Optional) Name of the ECS cluster. Required only if autoscaling is enabled | `string`| `null` | no |
| max\_cpu\_threshold                   | Threshold for max CPU usage | `string` | `85` | yes |
| min\_cpu\_threshold                   | Threshold for min CPU usage | `string` | `10` | yes |
| max\_cpu\_evaluation\_period          | The number of periods over which data is compared to the specified threshold for max cpu metric alarm. | `string` | `3` | yes |
| min\_cpu\_evaluation\_period          | The number of periods over which data is compared to the specified threshold for min cpu metric alarm. | `string` | `3` | yes |
| max\_cpu\_period                      | The period in seconds over which the specified statistic is applied for max cpu metric alarm. | `string` | `60` |  yes  |
| min\_cpu\_period                      | The period in seconds over which the specified statistic is applied for min cpu metric alarm. | `string` | `60` |  yes  |
| scale\_target\_max\_capacity          | The max capacity of the scalable target  | `number`          | `5`     |   yes   |
| scale\_target\_min\_capacity          | The min capacity of the scalable target  | `number`          | `1`     |   yes   |
| lb\_internal                          | (Optional) If true, public access to the S3 bucket will be blocked. | `bool`         | `false` |   no    |
| lb\_security\_groups                  | (Optional) A list of security group IDs to assign to the LB.        | `list(string)` | `[]`    |   no    |
| lb\_drop\_invalid\_header\_fields         | (Optional) Indicates whether HTTP headers with header fields that are not valid are removed by the load balancer (true) or routed to targets (false). The default is false. Elastic Load Balancing requires that message header names contain only alphanumeric characters and hyphens.                                   | `bool` | `false`     |   no    |
| lb\_idle\_timeout                        | (Optional) The time in seconds that the connection is allowed to be idle. Default: 60. | `number`         | `60` |   no    |
| lb\_enable\_deletion\_protection         | (Optional) If true, deletion of the load balancer will be disabled via the AWS API. This will prevent Terraform from deleting the load balancer. Defaults to false.  | `bool`         | `false` |   no    |
| lb\_enable\_cross\_zone\_load\_balancing | (Optional) If true, cross-zone load balancing of the load balancer will be enabled. Defaults to false. | `bool`         | `false` |   no    |
| lb\_enable\_http2                        | (Optional) Indicates whether HTTP/2 is enabled in the load balancer. Defaults to true.| `bool`         | `true` |   no    |
| lb\_ip\_address\_type                    | (Optional) The type of IP addresses used by the subnets for your load balancer. The possible values are ipv4 and dualstack. Defaults to ipv4 | `string`          | `ipv4`     |   no   |
| lb\_http\_ports                          | Map containing objects to define listeners behaviour based on type field. If type field is `forward`, include listener_port and the target_group_port. For `redirect` type, include listener port, host, path, port, protocol, query and status_code. For `fixed-response`, include listener_port, content_type, message_body and status_code | `map(any)`         | <pre>{<br>    default_http = {<br>        type              = "forward"<br>        listener_port     = 80<br>        target_group_port = 80<br>    }<br>}</pre> |   no    |
| lb\_http\_ingress\_cidr\_blocks          | List of CIDR blocks to allowed to access the Load Balancer through HTTP.        | `list(string)` | `["0.0.0.0/0"]`    |   no    |
| lb\_http\_ingress\_prefix\_list\_ids     | List of prefix list IDs blocks to allowed to access the Load Balancer through HTTP.        | `list(string)` | `[]`    |   no    |
| lb\_https\_ports                         | Map containing objects to define listeners behaviour based on type field. If type field is `forward`, include listener_port and the target_group_port. For `redirect` type, include listener port, host, path, port, protocol, query and status_code. For `fixed-response`, include listener_port, content_type, message_body and status_code. | `map(any)`         | <pre>{<br>    default_http = {<br>        type              = "forward"<br>        listener_port     = 443<br>        target_group_port = 443<br>    }<br>}</pre> |   no    |
| lb\_https\_ingress\_cidr\_blocks         | List of CIDR blocks to allowed to access the Load Balancer through HTTPS.        | `list(string)` | `["0.0.0.0/0"]`    |   no    |
| lb\_https\_ingress\_prefix\_list\_ids    | List of prefix list IDs blocks to allowed to access the Load Balancer through HTTPS.        | `list(string)` | `[]`    |   no    |
| lb\_deregistration\_delay                | (Optional) The amount time for Elastic Load Balancing to wait before changing the state of a deregistering target from draining to unused. The range is 0-3600 seconds. The default value is 300 seconds.        | `number` | `300`    |   no    |
| lb\_slow\_start                          | (Optional) The amount time for targets to warm up before the load balancer sends them a full share of requests. The range is 30-900 seconds or 0 to disable. The default value is 0 seconds.        | `number` | `0`    |   no    |
| lb\_load\_balancing\_algorithm\_type     | (Optional) Determines how the load balancer selects targets when routing requests. The value is round_robin or least_outstanding_requests. The default is round_robin.        | `string` | `round_robin`    |   no    |
| lb\_stickiness                           | (Optional) A Stickiness block. Provide three fields. type, the type of sticky sessions. The only current possible value is lb_cookie. cookie_duration, the time period, in seconds, during which requests from a client should be routed to the same target. After this time period expires, the load balancer-generated cookie is considered stale. The range is 1 second to 1 week (604800 seconds). The default value is 1 day (86400 seconds). enabled, boolean to enable / disable stickiness. Default is true.        | <pre>object({<br>    type            = string<br>    cookie_duration = string<br>    enabled         = bool<br>})</pre> | <pre>{<br>    type            = "lb_cookie"<br>    cookie_duration = 86400<br>    enabled         = true<br>}</pre>    |   no    |
| lb\_target\_group\_health\_check\_enabled  | (Optional) Indicates whether health checks are enabled. Defaults to true.        | `bool` | `true`    |   no    |
| lb\_target\_group\_health\_check\_interval | (Optional) The approximate amount of time, in seconds, between health checks of an individual target. Minimum value 5 seconds, Maximum value 300 seconds. Default 30 seconds.      | `number` | `30`    |   no    |
| lb\_target\_group\_health\_check\_path     | The destination for the health check request.        | `string` | `/`    |   no    |
| lb\_target\_group\_health\_check\_timeout  | (Optional) The amount of time, in seconds, during which no response means a failed health check. The range is 2 to 120 seconds, and the default is 5 seconds.        | `number` | `5`    |   no    |
| lb\_target\_group\_health\_check\_healthy\_threshold   | (Optional) The number of consecutive health checks successes required before considering an unhealthy target healthy. Defaults to 3.        | `number` | `3`    |   no    |
| lb\_target\_group\_health\_check\_unhealthy\_threshold | (Optional) The number of consecutive health check failures required before considering the target unhealthy. Defaults to 3.        | `number` | `3`    |   no    |
| lb\_target\_group\_health\_check\_matcher              | The HTTP codes to use when checking for a successful response from a target. You can specify multiple values (for example, \"200,202\") or a range of values (for example, \"200-299\"). Default is 200.        | `string` | `200`    |   no    |
| ssl\_policy                                          | (Optional) The name of the SSL Policy for the listener. . Required if var.https_ports is set.        | `string` | `null`    |   no    |
| default\_certificate\_arn                            | (Optional) The ARN of the default SSL server certificate. Required if var.https_ports is set.        | `string` | `null`    |   no    |
| additional\_certificates\_arn\_for\_https\_listeners | (Optional) List of SSL server certificate ARNs for HTTPS listener. Use it if you need to set additional certificates besides default_certificate_arn        | `list(any)` | `[]`    |   no    |
| block\_s3\_bucket\_public\_access     | (Optional) If true, public access to the S3 bucket will be blocked. | `bool`         | `false` |   no    |

## Outputs

| Name                                              | Description                                                                                |
| ------------------------------------------------- | ------------------------------------------------------------------------------------------ |
| aws\_ecs\_service\_service\_id                    | The Amazon Resource Name (ARN) that identifies the service. ARNs                           |
| aws\_ecs\_service\_service\_name                  | The name of the service. Names                                                             |
| aws\_ecs\_service\_service\_cluster               | The Amazon Resource Name (ARN) of cluster which the service runs on. IDs                   |
| aws\_ecs\_service\_service\_desired\_count        | The number of instances of the task definition ARNs                                        |
| ecs\_tasks\_sg\_id                                | $${var.name_prefix} ECS Tasks Security Group - The ID of the security group IDs            |
| ecs\_tasks\_sg\_arn                               | $${var.name_prefix} ECS Tasks Security Group - The ARN of the security group ARNs          |
| ecs\_tasks\_sg\_name                              | $${var.name_prefix} ECS Tasks Security Group - The name of the security group Names        |
| ecs\_tasks\_sg\_description                       | $${var.name_prefix} ECS Tasks Security Group - The description of the security group IDs   |
| aws\_lb\_lb\_id                                   | The ARN of the load balancer (matches arn).                                                |
| aws\_lb\_lb\_arn                                  | The ARN of the load balancer (matches id).                                                 |
| aws\_lb\_lb\_arn\_suffix                          | The ARN suffix for use with CloudWatch Metrics.                                            |
| aws\_lb\_lb\_dns\_name                            | The DNS name of the load balancer.                                                         |
| aws\_lb\_lb\_zone\_id                             | The canonical hosted zone ID of the load balancer (to be used in a Route 53 Alias record). |
| aws\_security\_group\_lb\_access\_sg\_id          | The ID of the security group                                                               |
| aws\_security\_group\_lb\_access\_sg\_arn         | The ARN of the security group                                                              |
| aws\_security\_group\_lb\_access\_sg\_vpc\_id     | The VPC ID.                                                                                |
| aws\_security\_group\_lb\_access\_sg\_owner\_id   | The owner ID.                                                                              |
| aws\_security\_group\_lb\_access\_sg\_name        | The name of the security group                                                             |
| aws\_security\_group\_lb\_access\_sg\_description | The description of the security group                                                      |
| aws\_security\_group\_lb\_access\_sg\_ingress     | The ingress rules.                                                                         |
| aws\_security\_group\_lb\_access\_sg\_egress      | The egress rules.                                                                          |
| lb\_http\_tgs\_ids                                | List of HTTP Target Groups IDs                                                             |
| lb\_http\_tgs\_arns                               | List of HTTP Target Groups ARNs                                                            |
| lb\_http\_tgs\_names                              | List of HTTP Target Groups Names                                                           |
| lb\_https\_tgs\_ids                               | List of HTTPS Target Groups IDs                                                            |
| lb\_https\_tgs\_arns                              | List of HTTPS Target Groups ARNs                                                           |
| lb\_https\_tgs\_names                             | List of HTTPS Target Groups Names                                                          |
| lb\_http\_listeners\_ids                          | List of HTTP Listeners IDs                                                                 |
| lb\_http\_listeners\_arns                         | List of HTTP Listeners ARNs                                                                |
| lb\_https\_listeners\_ids                         | List of HTTPS Listeners IDs                                                                |
| lb\_https\_listeners\_arns                        |List of HTTPS Listeners ARNs                                                                |
