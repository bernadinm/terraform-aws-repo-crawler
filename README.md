<!-- BEGIN_TF_DOCS -->
<!-- Documentation auto-generated by terraform-docs - DO NOT EDIT! -->
# Cyral Repo Crawler AWS module for Terraform

This is a Terraform module to install the Cyral Repo Crawler as an AWS
Lambda function, including all of its dependencies such as IAM permissions,
a DynamoDB cache, etc.

See the [examples](./examples) for usage details.

## Requirements

| Name | Version |
|------|---------|
| <a name="requirement_terraform"></a> [terraform](#requirement\_terraform) | >= 0.14 |
| <a name="requirement_aws"></a> [aws](#requirement\_aws) | ~> 4.0 |
| <a name="requirement_random"></a> [random](#requirement\_random) | ~> 3.1 |

## Providers

| Name | Version |
|------|---------|
| <a name="provider_aws"></a> [aws](#provider\_aws) | ~> 4.0 |
| <a name="provider_random"></a> [random](#provider\_random) | ~> 3.1 |

## Modules

No modules.

## Resources

| Name | Type |
|------|------|
| [aws_cloudwatch_event_rule.this](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/cloudwatch_event_rule) | resource |
| [aws_cloudwatch_event_target.this](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/cloudwatch_event_target) | resource |
| [aws_dynamodb_table.this](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/dynamodb_table) | resource |
| [aws_iam_role.this](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/iam_role) | resource |
| [aws_lambda_function.this](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/lambda_function) | resource |
| [aws_lambda_permission.this](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/lambda_permission) | resource |
| [aws_secretsmanager_secret.cyral_secret](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/secretsmanager_secret) | resource |
| [aws_secretsmanager_secret.repo_secret](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/secretsmanager_secret) | resource |
| [aws_secretsmanager_secret_version.cyral_secret_version](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/secretsmanager_secret_version) | resource |
| [aws_secretsmanager_secret_version.repo_secret_version](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/secretsmanager_secret_version) | resource |
| [aws_security_group.this](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/security_group) | resource |
| [random_id.this](https://registry.terraform.io/providers/hashicorp/random/latest/docs/resources/id) | resource |
| [aws_caller_identity.current](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/data-sources/caller_identity) | data source |
| [aws_iam_policy_document.assume_role_policy](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/data-sources/iam_policy_document) | data source |
| [aws_iam_policy_document.execution_policy](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/data-sources/iam_policy_document) | data source |
| [aws_partition.current](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/data-sources/partition) | data source |
| [aws_region.current](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/data-sources/region) | data source |

## Inputs

| Name | Description | Type | Default | Required |
|------|-------------|------|---------|:--------:|
| <a name="input_connection-string-args"></a> [connection-string-args](#input\_connection-string-args) | Optional database connection string options in `key=value` format:<br>    `opt1=val1`, `opt2=val2`, etc. Currently only works for PostgreSQL-like<br>    repos (i.e. Redshift, Denodo, or PostgreSQL), where this list gets<br>    serialized into a comma separated string. | `list(string)` | `[]` | no |
| <a name="input_control_plane_grpc_port"></a> [control\_plane\_grpc\_port](#input\_control\_plane\_grpc\_port) | The TCP/IP port for the Cyral Control Plane gRPC API (default: 443). | `number` | `443` | no |
| <a name="input_control_plane_host"></a> [control\_plane\_host](#input\_control\_plane\_host) | The host for the Cyral Control Plane API, e.g. tenant.cyral.com. | `string` | n/a | yes |
| <a name="input_control_plane_rest_port"></a> [control\_plane\_rest\_port](#input\_control\_plane\_rest\_port) | The TCP/IP port for the Cyral Control Plane REST API. (default: 443) | `number` | `443` | no |
| <a name="input_crawler_name"></a> [crawler\_name](#input\_crawler\_name) | The name of the Repo Crawler Lambda function. If omitted, it will default<br>    to `cyral-repo-crawler-<16 character random alphanumeric string>`. | `string` | `""` | no |
| <a name="input_crawler_version"></a> [crawler\_version](#input\_crawler\_version) | The version of the Cyral Repo Crawler to use, e.g. v1.2.3. | `string` | n/a | yes |
| <a name="input_cyral_client_id"></a> [cyral\_client\_id](#input\_cyral\_client\_id) | The client ID to connect to the Cyral API. | `string` | `""` | no |
| <a name="input_cyral_client_secret"></a> [cyral\_client\_secret](#input\_cyral\_client\_secret) | The client secret to connect to the Cyral API. | `string` | `""` | no |
| <a name="input_cyral_secret_arn"></a> [cyral\_secret\_arn](#input\_cyral\_secret\_arn) | ARN of the entry in AWS Secrets Manager that stores the secret containing<br>    the credentials for the Cyral API. If empty, cyral\_client\_id and<br>    cyral\_client\_secret variables must be provided, and a new secret will be<br>    created in AWS Secrets Manager. | `string` | `""` | no |
| <a name="input_dynamodb_cache_table_name_suffix"></a> [dynamodb\_cache\_table\_name\_suffix](#input\_dynamodb\_cache\_table\_name\_suffix) | The suffix for the DynamoDB table name used for the classification cache.<br>    The full table will be prefixed with the Lambda function name<br>    (default: cyralRepoCrawlerCache). | `string` | `"cyralRepoCrawlerCache"` | no |
| <a name="input_enable_account_discovery"></a> [enable\_account\_discovery](#input\_enable\_account\_discovery) | Configures the Crawler to run in account discovery mode, i.e., query and<br>    discover all existing user accounts in the database. | `bool` | `true` | no |
| <a name="input_enable_data_classification"></a> [enable\_data\_classification](#input\_enable\_data\_classification) | Configures the Crawler to run in data classification mode, i.e., sample and<br>    classify data according to a set of existing labels. | `bool` | `true` | no |
| <a name="input_oracle_service"></a> [oracle\_service](#input\_oracle\_service) | The Oracle service name. Omit if not configuring an Oracle repo. | `string` | `""` | no |
| <a name="input_repo_database"></a> [repo\_database](#input\_repo\_database) | The database on the repository that the repo crawler will connect to. | `string` | n/a | yes |
| <a name="input_repo_host"></a> [repo\_host](#input\_repo\_host) | The hostname or host address of the database instance. | `string` | n/a | yes |
| <a name="input_repo_max_open_conns"></a> [repo\_max\_open\_conns](#input\_repo\_max\_open\_conns) | Maximum number of open connections to the database. | `number` | `10` | no |
| <a name="input_repo_name"></a> [repo\_name](#input\_repo\_name) | The repository name on the Cyral Control Plane. | `string` | n/a | yes |
| <a name="input_repo_password"></a> [repo\_password](#input\_repo\_password) | The password to connect to the repository. | `string` | `""` | no |
| <a name="input_repo_port"></a> [repo\_port](#input\_repo\_port) | The port of the database service in the database instance. | `number` | n/a | yes |
| <a name="input_repo_sample_size"></a> [repo\_sample\_size](#input\_repo\_sample\_size) | Number of rows to sample from each table. | `number` | `5` | no |
| <a name="input_repo_secret_arn"></a> [repo\_secret\_arn](#input\_repo\_secret\_arn) | ARN of the entry in AWS Secrets Manager that stores the secret containing<br>    the credentials to connect to the repository. If empty, the repo\_username<br>    and repo\_password variables must be provided, and a new secret will be<br>    created in AWS Secrets Manager. | `string` | `""` | no |
| <a name="input_repo_type"></a> [repo\_type](#input\_repo\_type) | The repository type on the Cyral Control Plane. | `string` | n/a | yes |
| <a name="input_repo_username"></a> [repo\_username](#input\_repo\_username) | The username to connect to the repository. | `string` | `""` | no |
| <a name="input_schedule_expression"></a> [schedule\_expression](#input\_schedule\_expression) | Schedule expression to invoke the repo crawler. The default value<br>    represents a run schedule of every six hours. | `string` | `"cron(0 0/6 * * ? *)"` | no |
| <a name="input_snowflake_account"></a> [snowflake\_account](#input\_snowflake\_account) | The Snowflake account. Omit if not configuring a Snowflake repo. | `string` | `""` | no |
| <a name="input_snowflake_role"></a> [snowflake\_role](#input\_snowflake\_role) | The Snowflake role. Omit if not configuring a Snowflake repo. | `string` | `""` | no |
| <a name="input_snowflake_warehouse"></a> [snowflake\_warehouse](#input\_snowflake\_warehouse) | The Snowflake warehouse. Omit if not configuring a Snowflake repo. | `string` | `""` | no |
| <a name="input_subnet_ids"></a> [subnet\_ids](#input\_subnet\_ids) | The subnets that the Repo Crawler Lambda function will be deployed to. All<br>    subnets must be able to reach both the Cyral Control Plane and the database<br>    being crawled. These subnets must also support communication with<br>    CloudWatch and Secrets Manager, therefore outbound internet access is<br>    likely required. | `list(string)` | n/a | yes |
| <a name="input_timeout"></a> [timeout](#input\_timeout) | The timeout of the Repo Crawler Lambda function, in seconds. | `number` | `300` | no |
| <a name="input_vpc_id"></a> [vpc\_id](#input\_vpc\_id) | The VPC the lambda will be attached to. | `string` | n/a | yes |

## Outputs

| Name | Description |
|------|-------------|
| <a name="output_repo_crawler_lambda_function_arn"></a> [repo\_crawler\_lambda\_function\_arn](#output\_repo\_crawler\_lambda\_function\_arn) | The Amazon Resource Name (ARN) of the Repo Crawler Lambda function. |
<!-- END_TF_DOCS -->