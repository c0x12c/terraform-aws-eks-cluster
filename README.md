# AWS EKS Terraform module

Terraform module which creates Amazon EKS (Kubernetes) resources

This module will create the components below

- Default system applications like: coredns, metrics-server,
  aws-load-balancer-controller, nginx-ingress-controller
- Associated IAM components for running the cluster
- Default nodes and Fargate profile for running pods
- EFS system for mounting volumes

## Usage
### Create EKS cluster

```hcl
module "eks" {
  source  = "c0x12c/eks-cluster/aws"
  version = "1.0.1"

  region          = "us-west-2"
  environment     = "test"
  cluster_version = "1.28"
  name            = "example"
  enabled_api_and_config_map = true

  # networking
  security_group_ids = []
  vpc_id   = "vpc-123456"
  vpc_cidr = "0.0.0.0/0"
  public_subnet_ids = ["subnet-abcde012"]
  private_subnet_ids = ["subnet-abcde014"]

  # feature flags
  enabled_cluster_log_types = []
  enabled_cloudwatch_logging = false
  create_fargate_profile     = true
  enabled_datadog_agent      = true
  enabled_efs = true

  # fargate
  fargate_profiles = {
    default = {
      selectors = [
        {
          namespace = "*"
        }
      ]
    }
  }

  fargate_timeouts = {
    create = "20m"
    delete = "20m"
  }

  custom_namespaces = ["jenkins", "datadog", "service-bot"]
  compute_type = "fargate"

  # custom RBAC
  administrator_role_arn = null
  aws_auth_users = []
}
```

## Examples

- [Example Fargate](./examples/fargate/)
- [Example Managed nodes](./examples/managed-nodes/)

<!-- BEGIN_TF_DOCS -->

## Requirements

| Name | Version |
|------|---------|
| <a name="requirement_terraform"></a> [terraform](#requirement\_terraform) | >= 1.9.8 |
| <a name="requirement_aws"></a> [aws](#requirement\_aws) | >= 5.83 |
| <a name="requirement_kubernetes"></a> [kubernetes](#requirement\_kubernetes) | >= 2.33.0 |
| <a name="requirement_tls"></a> [tls](#requirement\_tls) | >= 4.0 |

## Providers

| Name | Version |
|------|---------|
| <a name="provider_aws"></a> [aws](#provider\_aws) | 6.3.0 |
| <a name="provider_kubernetes"></a> [kubernetes](#provider\_kubernetes) | 2.37.1 |
| <a name="provider_tls"></a> [tls](#provider\_tls) | 4.1.0 |

## Modules

| Name | Source | Version |
|------|--------|---------|
| <a name="module_cloudwatch_logging"></a> [cloudwatch\_logging](#module\_cloudwatch\_logging) | c0x12c/eks-cloudwatch-logging-fargate/aws | 1.0.0 |
| <a name="module_datadog_rbac"></a> [datadog\_rbac](#module\_datadog\_rbac) | c0x12c/eks-datadog-rbac/aws | 1.0.0 |
| <a name="module_efs"></a> [efs](#module\_efs) | c0x12c/eks-efs/aws | 1.0.0 |
| <a name="module_eks_managed_node_group"></a> [eks\_managed\_node\_group](#module\_eks\_managed\_node\_group) | c0x12c/eks-managed-node-group/aws | 1.0.0 |
| <a name="module_fargate_profile"></a> [fargate\_profile](#module\_fargate\_profile) | c0x12c/eks-fargate-profile/aws | 1.0.0 |

## Resources

| Name | Type |
|------|------|
| [aws_eks_access_entry.auth_role](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/eks_access_entry) | resource |
| [aws_eks_access_entry.fargate_profile](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/eks_access_entry) | resource |
| [aws_eks_access_entry.node_role](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/eks_access_entry) | resource |
| [aws_eks_access_policy_association.auth_role](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/eks_access_policy_association) | resource |
| [aws_eks_access_policy_association.node_role](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/eks_access_policy_association) | resource |
| [aws_eks_addon.coredns_ec2](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/eks_addon) | resource |
| [aws_eks_addon.coredns_fargate](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/eks_addon) | resource |
| [aws_eks_addon.efs_csi_driver](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/eks_addon) | resource |
| [aws_eks_addon.kube_proxy](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/eks_addon) | resource |
| [aws_eks_addon.vpc_cni](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/eks_addon) | resource |
| [aws_eks_cluster.master](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/eks_cluster) | resource |
| [aws_iam_group.eks_users](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/iam_group) | resource |
| [aws_iam_group_policy.eks_users](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/iam_group_policy) | resource |
| [aws_iam_instance_profile.node](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/iam_instance_profile) | resource |
| [aws_iam_openid_connect_provider.eks](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/iam_openid_connect_provider) | resource |
| [aws_iam_policy.custom_worker](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/iam_policy) | resource |
| [aws_iam_policy.k8s_masters](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/iam_policy) | resource |
| [aws_iam_role.auth_role](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/iam_role) | resource |
| [aws_iam_role.efs_csi_driver_role](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/iam_role) | resource |
| [aws_iam_role.master](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/iam_role) | resource |
| [aws_iam_role.node](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/iam_role) | resource |
| [aws_iam_role_policy.eks_policy](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/iam_role_policy) | resource |
| [aws_iam_role_policy_attachment.efs_csi_driver_policy_attachment](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/iam_role_policy_attachment) | resource |
| [aws_iam_role_policy_attachment.master_policy_attachments](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/iam_role_policy_attachment) | resource |
| [aws_iam_role_policy_attachment.node_policy_attachments](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/iam_role_policy_attachment) | resource |
| [aws_security_group.cluster](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/security_group) | resource |
| [kubernetes_config_map_v1_data.aws_auth](https://registry.terraform.io/providers/hashicorp/kubernetes/latest/docs/resources/config_map_v1_data) | resource |
| [kubernetes_namespace.fargate](https://registry.terraform.io/providers/hashicorp/kubernetes/latest/docs/resources/namespace) | resource |
| [aws_caller_identity.current](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/data-sources/caller_identity) | data source |
| [aws_eks_addon_version.coredns_latest](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/data-sources/eks_addon_version) | data source |
| [aws_eks_addon_version.efs_csi_driver_latest](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/data-sources/eks_addon_version) | data source |
| [aws_eks_addon_version.kube_proxy_latest](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/data-sources/eks_addon_version) | data source |
| [aws_eks_addon_version.vpc_cni_latest](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/data-sources/eks_addon_version) | data source |
| [aws_iam_policy_document.assume_role_policy](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/data-sources/iam_policy_document) | data source |
| [aws_iam_policy_document.auth_policy](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/data-sources/iam_policy_document) | data source |
| [aws_iam_policy_document.custom_worker_policy](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/data-sources/iam_policy_document) | data source |
| [aws_iam_policy_document.eks_assume_role](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/data-sources/iam_policy_document) | data source |
| [aws_iam_policy_document.k8s_masters_policy](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/data-sources/iam_policy_document) | data source |
| [aws_iam_policy_document.node_assume_role_policy](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/data-sources/iam_policy_document) | data source |
| [aws_iam_policy_document.users_policy](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/data-sources/iam_policy_document) | data source |
| [aws_region.current](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/data-sources/region) | data source |
| [tls_certificate.eks](https://registry.terraform.io/providers/hashicorp/tls/latest/docs/data-sources/certificate) | data source |

## Inputs

| Name | Description | Type | Default | Required |
|------|-------------|------|---------|:--------:|
| <a name="input_addons_coredns_version"></a> [addons\_coredns\_version](#input\_addons\_coredns\_version) | The version of the CoreDNS addon, latest by default | `string` | `null` | no |
| <a name="input_addons_efs_csi_driver_version"></a> [addons\_efs\_csi\_driver\_version](#input\_addons\_efs\_csi\_driver\_version) | The version of the EFS CSI Driver addon, latest by default | `string` | `null` | no |
| <a name="input_addons_kube_proxy_version"></a> [addons\_kube\_proxy\_version](#input\_addons\_kube\_proxy\_version) | The version of the kube-proxy addon, latest by default | `string` | `null` | no |
| <a name="input_addons_vpc_cni_version"></a> [addons\_vpc\_cni\_version](#input\_addons\_vpc\_cni\_version) | The version of the VPC CNI addon, latest by default | `string` | `null` | no |
| <a name="input_administrator_role_arn"></a> [administrator\_role\_arn](#input\_administrator\_role\_arn) | AWS Administrator role arn for mapping with K8s RBAC | `string` | `null` | no |
| <a name="input_authentication_mode"></a> [authentication\_mode](#input\_authentication\_mode) | The authentication mode, allowed values are CONFIG\_MAP, API, or API\_AND\_CONFIG\_MAP. | `string` | `"API"` | no |
| <a name="input_aws_auth_accounts"></a> [aws\_auth\_accounts](#input\_aws\_auth\_accounts) | AWS accounts for authenticating with Kubernetes | `list(any)` | `[]` | no |
| <a name="input_aws_auth_users"></a> [aws\_auth\_users](#input\_aws\_auth\_users) | AWS users for authenticating with Kubernetes | `list(any)` | `[]` | no |
| <a name="input_cluster_version"></a> [cluster\_version](#input\_cluster\_version) | The Kubernetes version for creating the cluster. | `string` | `"1.32"` | no |
| <a name="input_compute_type"></a> [compute\_type](#input\_compute\_type) | The type of compute resources to be used, e.g., 'ec2' or 'fargate'. | `string` | `"ec2"` | no |
| <a name="input_coredns"></a> [coredns](#input\_coredns) | Configuration for CoreDNS add-on | <pre>object({<br/>    replica_count = optional(number, 1)<br/>    node_selector = optional(map(string), null)<br/>    tolerations = optional(list(object({<br/>      key      = string<br/>      operator = string<br/>      value    = optional(string)<br/>      effect   = optional(string)<br/>    })), [])<br/>  })</pre> | <pre>{<br/>  "node_selector": null,<br/>  "replica_count": 1,<br/>  "tolerations": []<br/>}</pre> | no |
| <a name="input_create_fargate_profile"></a> [create\_fargate\_profile](#input\_create\_fargate\_profile) | Specify whether creating the Fargate profile for running pods. | `bool` | `false` | no |
| <a name="input_create_fargate_profile_access_entry"></a> [create\_fargate\_profile\_access\_entry](#input\_create\_fargate\_profile\_access\_entry) | Create access entry for Fargate profile | `bool` | `false` | no |
| <a name="input_custom_namespaces"></a> [custom\_namespaces](#input\_custom\_namespaces) | Custom namespaces to be created during initialization | `list(string)` | `[]` | no |
| <a name="input_custom_service_accounts"></a> [custom\_service\_accounts](#input\_custom\_service\_accounts) | Map of service account names for binding with Datadog.<br/>Each key represents a namespace, and the value is a list of service account names.<br/>  {<br/>    namespace = ["service-account1", "service-account2] <br/>  } | `map(list(string))` | `{}` | no |
| <a name="input_datadog_agent_cluster_role_name"></a> [datadog\_agent\_cluster\_role\_name](#input\_datadog\_agent\_cluster\_role\_name) | Name of the ClusterRole to create in order to configure Datadog Agents | `string` | `"datadog-agent"` | no |
| <a name="input_default_service_account"></a> [default\_service\_account](#input\_default\_service\_account) | Default service account name for binding with Datadog | `string` | `"default"` | no |
| <a name="input_efs_backup_policy_status"></a> [efs\_backup\_policy\_status](#input\_efs\_backup\_policy\_status) | Enable/disable backup for EFS Filesystem.  Value should be ENABLE/DISABLED.  Defaults to DISABLED | `string` | `"DISABLED"` | no |
| <a name="input_efs_csi"></a> [efs\_csi](#input\_efs\_csi) | Configuration for EFS CSI add-on | <pre>object({<br/>    replica_count = optional(number, 1)<br/>    node_selector = optional(map(string), null)<br/>    tolerations = optional(list(object({<br/>      key      = string<br/>      operator = string<br/>      value    = optional(string)<br/>      effect   = optional(string)<br/>    })), [])<br/>  })</pre> | <pre>{<br/>  "node_selector": null,<br/>  "replica_count": 1,<br/>  "tolerations": []<br/>}</pre> | no |
| <a name="input_efs_filesystem_name"></a> [efs\_filesystem\_name](#input\_efs\_filesystem\_name) | To specify the name of efs filesystem in case overwrite the default one | `string` | `null` | no |
| <a name="input_efs_lifecycle_policy"></a> [efs\_lifecycle\_policy](#input\_efs\_lifecycle\_policy) | Lifecycle Policy for the EFS Filesystem | <pre>list(object({<br/>    transition_to_ia                    = string<br/>    transition_to_primary_storage_class = string<br/>  }))</pre> | `[]` | no |
| <a name="input_efs_storage_class_name"></a> [efs\_storage\_class\_name](#input\_efs\_storage\_class\_name) | n/a | `string` | `"efs"` | no |
| <a name="input_enable_access_config"></a> [enable\_access\_config](#input\_enable\_access\_config) | Enable or disable access configuration for the Kubernetes cluster. | `bool` | `false` | no |
| <a name="input_enabled_cloudwatch_logging"></a> [enabled\_cloudwatch\_logging](#input\_enabled\_cloudwatch\_logging) | Enable logging for Kubernetes Pods through built in EKS Fargate Firelens | `bool` | `false` | no |
| <a name="input_enabled_cluster_log_types"></a> [enabled\_cluster\_log\_types](#input\_enabled\_cluster\_log\_types) | Enabled logging types for EKS Control Plane | `list(string)` | <pre>[<br/>  "api",<br/>  "audit",<br/>  "authenticator",<br/>  "controllerManager",<br/>  "scheduler"<br/>]</pre> | no |
| <a name="input_enabled_datadog_agent"></a> [enabled\_datadog\_agent](#input\_enabled\_datadog\_agent) | Enable datadog integration RBAC creation | `bool` | `false` | no |
| <a name="input_enabled_efs"></a> [enabled\_efs](#input\_enabled\_efs) | Enable EFS creation for persistence volumes | `bool` | `false` | no |
| <a name="input_enabled_endpoint_private_access"></a> [enabled\_endpoint\_private\_access](#input\_enabled\_endpoint\_private\_access) | Enable private access for the Kubernetes API server endpoint. | `bool` | `true` | no |
| <a name="input_enabled_endpoint_public_access"></a> [enabled\_endpoint\_public\_access](#input\_enabled\_endpoint\_public\_access) | Enable public access for the Kubernetes API server endpoint. | `bool` | `true` | no |
| <a name="input_enabled_karpenter"></a> [enabled\_karpenter](#input\_enabled\_karpenter) | Whether to integrate Karpenter to cluster | `bool` | `false` | no |
| <a name="input_environment"></a> [environment](#input\_environment) | The environment name, will be used in components's name. | `string` | n/a | yes |
| <a name="input_fargate_profiles"></a> [fargate\_profiles](#input\_fargate\_profiles) | Configuration block(s) for selecting Kubernetes Pods to execute with this Fargate Profile | `any` | `{}` | no |
| <a name="input_fargate_timeouts"></a> [fargate\_timeouts](#input\_fargate\_timeouts) | Default timeout attributes. | <pre>object({<br/>    create = string<br/>    delete = string<br/>  })</pre> | <pre>{<br/>  "create": "20m",<br/>  "delete": "20m"<br/>}</pre> | no |
| <a name="input_filesystem_encrypted"></a> [filesystem\_encrypted](#input\_filesystem\_encrypted) | Enable encryption for EFS | `bool` | `true` | no |
| <a name="input_filesystem_performance_mode"></a> [filesystem\_performance\_mode](#input\_filesystem\_performance\_mode) | The file system performance mode. | `string` | `"generalPurpose"` | no |
| <a name="input_filesystem_throughput_mode"></a> [filesystem\_throughput\_mode](#input\_filesystem\_throughput\_mode) | Throughput mode for the file system. | `string` | `"bursting"` | no |
| <a name="input_name"></a> [name](#input\_name) | The prefix name that will be used in cluster and components's name. | `string` | n/a | yes |
| <a name="input_node_groups"></a> [node\_groups](#input\_node\_groups) | Key-value mapping of Kubernetes node groups attributes | <pre>map(object({<br/>    node_group_name = string<br/>    disk_size       = number<br/>    instance_types  = list(string)<br/>    desired_size    = number<br/>    max_size        = number<br/>    min_size        = number<br/>    labels          = map(string)<br/>    taints = list(object({<br/>      key    = string<br/>      value  = string<br/>      effect = string<br/>    }))<br/>  }))</pre> | `{}` | no |
| <a name="input_node_repair_config"></a> [node\_repair\_config](#input\_node\_repair\_config) | The node auto repair configuration for the node group | `bool` | `true` | no |
| <a name="input_private_subnet_ids"></a> [private\_subnet\_ids](#input\_private\_subnet\_ids) | The private subnet IDs in the associated VPC | `list(string)` | n/a | yes |
| <a name="input_public_access_cidrs"></a> [public\_access\_cidrs](#input\_public\_access\_cidrs) | List of CIDR blocks allowed for public access to the Kubernetes API server endpoint. | `list(string)` | <pre>[<br/>  "0.0.0.0/0"<br/>]</pre> | no |
| <a name="input_public_subnet_ids"></a> [public\_subnet\_ids](#input\_public\_subnet\_ids) | The public subnet IDs in the associated VPC | `list(string)` | n/a | yes |
| <a name="input_region"></a> [region](#input\_region) | Region of the EKS cluster. | `string` | `null` | no |
| <a name="input_security_group_ids"></a> [security\_group\_ids](#input\_security\_group\_ids) | Security group IDs that will be used in additional to the default ones. | `list(string)` | n/a | yes |
| <a name="input_update_config"></a> [update\_config](#input\_update\_config) | Configuration block of settings for max unavailable resources during node group updates | `map(string)` | <pre>{<br/>  "max_unavailable_percentage": 33<br/>}</pre> | no |
| <a name="input_vpc_cidr"></a> [vpc\_cidr](#input\_vpc\_cidr) | VPC CIDR for creating default security groups | `string` | n/a | yes |
| <a name="input_vpc_id"></a> [vpc\_id](#input\_vpc\_id) | The ID of the VPC that the cluster will be created on. | `string` | n/a | yes |

## Outputs

| Name | Description |
|------|-------------|
| <a name="output_aws_eks_cluster"></a> [aws\_eks\_cluster](#output\_aws\_eks\_cluster) | The EKS cluster master details, including API server endpoint and certificate authority |
| <a name="output_aws_eks_cluster_auth_data"></a> [aws\_eks\_cluster\_auth\_data](#output\_aws\_eks\_cluster\_auth\_data) | The ConfigMap data for managing EKS cluster authentication |
| <a name="output_aws_iam_instance_profile_node"></a> [aws\_iam\_instance\_profile\_node](#output\_aws\_iam\_instance\_profile\_node) | The instance profile associated with the EKS worker nodes |
| <a name="output_aws_iam_role_node"></a> [aws\_iam\_role\_node](#output\_aws\_iam\_role\_node) | The IAM role assigned to the EKS worker nodes for managing permissions |
| <a name="output_aws_security_group_cluster"></a> [aws\_security\_group\_cluster](#output\_aws\_security\_group\_cluster) | The security group applied to the EKS cluster for network control |
| <a name="output_datadog_agent_cluster_role_name"></a> [datadog\_agent\_cluster\_role\_name](#output\_datadog\_agent\_cluster\_role\_name) | Name of the ClusterRole to create in order to configure Datadog Agents |
| <a name="output_efs"></a> [efs](#output\_efs) | The Amazon EFS (Elastic File System) configuration for the cluster, if available |
| <a name="output_eks_default_auth_role_arn"></a> [eks\_default\_auth\_role\_arn](#output\_eks\_default\_auth\_role\_arn) | The ARN of the IAM role used for default EKS authentication |
| <a name="output_fargate_profile"></a> [fargate\_profile](#output\_fargate\_profile) | Details of the Fargate profile configured for the EKS cluster |
| <a name="output_oidc_provider"></a> [oidc\_provider](#output\_oidc\_provider) | The OpenID Connect (OIDC) provider associated with the EKS cluster for IAM roles |

<!-- END_TF_DOCS -->
