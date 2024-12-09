# Ansible Role: azure_network_infrastructure

The `azure_network_infrastructure` Ansible role is designed to configure and deploy the necessary network infrastructure for Cisco SD-WAN services in the Azure cloud environment. It includes creating and managing the resource group, virtual network, subnets, and network security groups required for a secure and operational SD-WAN deployment.

NOTE: This role should be executed on localhost since it involves making API requests to Azure directly from the control machine.

## Role Description

The `azure_network_infrastructure` role performs the following actions to set up the Azure network infrastructure for SD-WAN:

- Verifies if the user session with Azure is active.
- Ensures that all required variables for the network infrastructure deployment are provided.
- Adjusts the resource prefix to comply with Azure naming conventions.
- Prepares a directory to store the results of the network infrastructure setup.
- Includes tasks to create and manage network resources such as virtual networks, subnets, and security groups.

## Requirements

- The `cisco.sdwan_deployment` collection installed.
- Ansible 2.16 or higher.
- Ansible Azure modules (`azure.azcollection` collection) installed.
- Azure CLI configured with the necessary permissions to manage Azure network resources.

## Dependencies

- A role named cisco.sdwan_deployment.common`  that contains tasks for verifying Azure dependencies, user sessions, and asserting required variables.

## Role Variables

### Defaults (`defaults/main.yml`)

Variables with default values that the user may need to override:

- `organization_name`: The name of the organization deploying the infrastructure. This must be defined by the user.
- `az_location`: The Azure location where the network resources will be deployed. Must be specified by the user.
- `az_tag_creator`: Tag used to identify the creator of the resources, defaults to the organization name.
- `az_resources_prefix`: Prefix for the Azure resources, defaulting to the organization name but customizable by the user.
- `az_resource_group`: Default name for the Azure resource group.
- `az_virtual_network`, `az_vn_address_prefixes_cidr`, `az_subnets`: Default configurations for the Azure virtual network and subnets.
- `az_network_security_group`: Default name for the Azure network security group.
- `az_allowed_subnets`: VPN subnets allowed to connect to Azure External IPs. Should be defined by the user.

### Vars (`vars/main.yml`)

- `results_dir`: The directory where the results of the network deployment will be stored.

### Required Variables

- `organization_name`: The organization's name, used as a prefix for naming Azure resources.
- `az_location`: The geographical location in Azure where the resources will be deployed.
- `az_subnets`: A list of subnet configurations within the Azure Virtual Network.
- `az_allowed_subnets`: Subnets permitted to access the Azure resources.

## Example Playbook

See [Example playbooks](https://github.com/cisco-en-programmability/ansible-collection-sdwan-deployment/tree/main/playbooks).

These playbook reuse configuration files that might be used as example for your configuration

## License

"GPL-3.0-only"

## Author Information

This role was created by Arkadiusz Cichon <acichon@cisco.com>
