# Terraform-azure-network-security-group

# Terraform Azure Cloud Network-Security-Group Module

## Table of Contents
- [Introduction](#introduction)
- [Usage](#usage)
- [Examples](#examples)
- [Authors](#authors)
- [License](#license)
- [Inputs](#inputs)
- [Outputs](#outputs)

## Introduction

This Terraform configuration sets up Azure infrastructure, including a resource group, virtual network, subnet, and network security group (NSG) with specified configurations.

## Usage

To get started, make sure you have configured your Azure provider. You can use the following code as a starting point:

# Example: Network-Security-Group

```hcl
module "network_security_group" {
  source                  = "cypik/network-security-group/azure"
  version                 = "1.0.1"
  name                    = "app"
  environment             = "test"
  resource_group_name     = module.resource_group.resource_group_name
  resource_group_location = module.resource_group.resource_group_location
  subnet_ids              = [module.subnet.default_subnet_id]
  inbound_rules = [
    {
      name                       = "ssh"
      priority                   = 101
      access                     = "Allow"
      protocol                   = "Tcp"
      source_address_prefix      = "10.20.0.0/32"
      source_port_range          = "*"
      destination_address_prefix = "0.0.0.0/0"
      destination_port_range     = "22"
      description                = "SSH allowed port"
    },
    {
      name                       = "https"
      priority                   = 102
      access                     = "Allow"
      protocol                   = "*"
      source_address_prefix      = "VirtualNetwork"
      source_port_range          = "80,443"
      destination_address_prefix = "0.0.0.0/0"
      destination_port_range     = "22"
      description                = "HTTPS allowed ports"
    }
  ]
}
```

Make sure to configure the provider block with your Azure credentials or use other authentication methods. Adjust the variables according to your requirements.

## Examples
For detailed examples on how to use this module, please refer to the [example](https://github.com/cypik/terraform-azure-network-security-group/blob/master/_example) directory within this repository.

## Authors
Your Name Replace **MIT** and **Cypik** with the appropriate license and your information. Feel free to expand this README with additional details or usage instructions as needed for your specific use case.

## License
This project is licensed under the **MIT** License - see the [LICENSE](https://github.com/cypik/terraform-azure-network-security-group/blob/master/LICENSE) file for details.

<!-- BEGIN_TF_DOCS -->
## Requirements

| Name | Version |
|------|---------|
| <a name="requirement_terraform"></a> [terraform](#requirement\_terraform) | >= 1.6.6 |
| <a name="requirement_azurerm"></a> [azurerm](#requirement\_azurerm) | >=3.87.0 |

## Providers

| Name | Version |
|------|---------|
| <a name="provider_azurerm"></a> [azurerm](#provider\_azurerm) | >=3.87.0 |

## Modules

| Name | Source | Version |
|------|--------|---------|
| <a name="module_labels"></a> [labels](#module\_labels) | cypik/labels/azure | 1.0.1 |

## Resources

| Name | Type |
|------|------|
| [azurerm_network_security_group.nsg](https://registry.terraform.io/providers/hashicorp/azurerm/latest/docs/resources/network_security_group) | resource |
| [azurerm_network_security_rule.inbound](https://registry.terraform.io/providers/hashicorp/azurerm/latest/docs/resources/network_security_rule) | resource |
| [azurerm_network_security_rule.outbound](https://registry.terraform.io/providers/hashicorp/azurerm/latest/docs/resources/network_security_rule) | resource |
| [azurerm_network_watcher_flow_log.nsg_flow_logs](https://registry.terraform.io/providers/hashicorp/azurerm/latest/docs/resources/network_watcher_flow_log) | resource |
| [azurerm_subnet_network_security_group_association.example](https://registry.terraform.io/providers/hashicorp/azurerm/latest/docs/resources/subnet_network_security_group_association) | resource |

## Inputs

| Name | Description | Type | Default | Required |
|------|-------------|------|---------|:--------:|
| <a name="input_create"></a> [create](#input\_create) | Used when creating the Resource Group. | `string` | `"30m"` | no |
| <a name="input_delete"></a> [delete](#input\_delete) | Used when deleting the Resource Group. | `string` | `"30m"` | no |
| <a name="input_enable_flow_logs"></a> [enable\_flow\_logs](#input\_enable\_flow\_logs) | Flag to be set true when network security group flow logging feature is to be enabled. | `bool` | `false` | no |
| <a name="input_enable_traffic_analytics"></a> [enable\_traffic\_analytics](#input\_enable\_traffic\_analytics) | Boolean flag to enable/disable traffic analytics. | `bool` | `false` | no |
| <a name="input_enabled"></a> [enabled](#input\_enabled) | Set to false to prevent the module from creating any resources. | `bool` | `true` | no |
| <a name="input_environment"></a> [environment](#input\_environment) | Environment (e.g. `prod`, `dev`, `staging`). | `string` | `""` | no |
| <a name="input_flow_log_retention_policy_days"></a> [flow\_log\_retention\_policy\_days](#input\_flow\_log\_retention\_policy\_days) | The number of days to retain flow log records. | `number` | `100` | no |
| <a name="input_flow_log_retention_policy_enabled"></a> [flow\_log\_retention\_policy\_enabled](#input\_flow\_log\_retention\_policy\_enabled) | Boolean flag to enable/disable retention. | `bool` | `false` | no |
| <a name="input_flow_log_storage_account_id"></a> [flow\_log\_storage\_account\_id](#input\_flow\_log\_storage\_account\_id) | The id of storage account in which flow logs will be received. Note: Currently, only standard-tier storage accounts are supported. | `string` | `null` | no |
| <a name="input_flow_log_version"></a> [flow\_log\_version](#input\_flow\_log\_version) | The version (revision) of the flow log. Possible values are 1 and 2. | `number` | `1` | no |
| <a name="input_inbound_rules"></a> [inbound\_rules](#input\_inbound\_rules) | List of objects that represent the configuration of each inbound rule. | `any` | `[]` | no |
| <a name="input_label_order"></a> [label\_order](#input\_label\_order) | Label order, e.g. sequence of application name and environment `name`,`environment`,'attribute' [`webserver`,`qa`,`devops`,`public`,] . | `list(any)` | <pre>[<br>  "name",<br>  "environment"<br>]</pre> | no |
| <a name="input_log_analytics_workspace_id"></a> [log\_analytics\_workspace\_id](#input\_log\_analytics\_workspace\_id) | log analytics workspace id to pass it to destination details of diagnosys setting of NSG. | `string` | `null` | no |
| <a name="input_log_analytics_workspace_resource_id"></a> [log\_analytics\_workspace\_resource\_id](#input\_log\_analytics\_workspace\_resource\_id) | The resource ID of the attached log analytics workspace. | `string` | `null` | no |
| <a name="input_managedby"></a> [managedby](#input\_managedby) | ManagedBy, eg 'cypik'. | `string` | `"Cypik"` | no |
| <a name="input_name"></a> [name](#input\_name) | Name  (e.g. `app` or `cluster`). | `string` | `""` | no |
| <a name="input_network_watcher_name"></a> [network\_watcher\_name](#input\_network\_watcher\_name) | The name of the Network Watcher. Changing this forces a new resource to be created. | `string` | `null` | no |
| <a name="input_outbound_rules"></a> [outbound\_rules](#input\_outbound\_rules) | List of objects that represent the configuration of each outbound rule. | `any` | `[]` | no |
| <a name="input_read"></a> [read](#input\_read) | Used when retrieving the Resource Group. | `string` | `"5m"` | no |
| <a name="input_repository"></a> [repository](#input\_repository) | Terraform current module repo | `string` | `"https://github.com/cypik/terraform-azure-network-security-group"` | no |
| <a name="input_resource_group_location"></a> [resource\_group\_location](#input\_resource\_group\_location) | The Location of the resource group where to create the network security group. | `string` | n/a | yes |
| <a name="input_resource_group_name"></a> [resource\_group\_name](#input\_resource\_group\_name) | The name of the resource group in which to create the network security group. | `string` | n/a | yes |
| <a name="input_subnet_ids"></a> [subnet\_ids](#input\_subnet\_ids) | The ID of the Subnet. Changing this forces a new resource to be created. | `list(string)` | `[]` | no |
| <a name="input_update"></a> [update](#input\_update) | Used when updating the Resource Group. | `string` | `"30m"` | no |

## Outputs

| Name | Description |
|------|-------------|
| <a name="output_id"></a> [id](#output\_id) | The network security group configuration ID. |
<!-- END_TF_DOCS -->