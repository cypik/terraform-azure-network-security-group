# terraform-azure-network-security-group
# Azure Infrastructure Terraform Configuration

## Table of Contents

- [Introduction](#introduction)
- [Usage](#usage)
- [Module Inputs](#module-inputs)
- [Examples](#examples)
- [Author](#author)
- [License](#license)

## Introduction

This Terraform configuration sets up Azure infrastructure, including a resource group, virtual network, subnet, and network security group (NSG) with specified configurations.

## Usage

To get started, make sure you have configured your Azure provider. You can use the following code as a starting point:

```hcl

module "vnet" {
  source              = "https://github.com/opz0/terraform-azure-network-security-group.git"
  name                = "app"
  environment         = "test"
  resource_group_name = module.resource_group.resource_group_name
  location            = module.resource_group.resource_group_location
  address_space       = "10.0.0.0/16"
}

module "subnet" {
  source              = "git::git@github.com:opz0/terraform-azure-subnet.git?ref=master"
  name                = "app"
  environment         = "test"
  resource_group_name = module.resource_group.resource_group_name
  location            = module.resource_group.resource_group_location
  virtual_network_name = module.vnet.vnet_name[0]
  subnet_names        = ["subnet1"]
  subnet_prefixes     = ["10.0.1.0/24"]
  enable_route_table  = true
  route_table_name    = "default_subnet"
  routes = [
    {
      name           = "rt-test"
      address_prefix = "0.0.0.0/0"
      next_hop_type  = "Internet"
    }
  ]
}

module "network_security_group" {
  source                  = "./../"  # Update with the correct path or Git repository
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

## Module Inputs
- name (string): The name of the infrastructure components.
- environment (string): The environment in which the infrastructure is being created.
- location (string): The Azure region for the resources.
- address_space (string): The address space for the virtual network.
- subnet_names (list of strings): The names of the subnets.
-subnet_prefixes (list of strings): The CIDR prefixes for the subnets.
- enable_route_table (bool): Flag indicating whether to enable a route table.
- route_table_name (string): The name of the route table.
- routes (list of objects): The routes for the route table.
- inbound_rules (list of objects): The inbound rules for the network security group.
## Examples
- For detailed examples on how to use this module, please refer to the 'examples' directory within this repository.

  ## Author
Your Name Replace '[License Name]' and '[Your Name]' with the appropriate license and your information. Feel free to expand this README with additional details or usage instructions as needed for your specific use case.

## License
This project is licensed under the MIT License - see the [LICENSE](https://github.com/opz0/terraform-azure-network-security-group/blob/readme/LICENSE) file for details.
