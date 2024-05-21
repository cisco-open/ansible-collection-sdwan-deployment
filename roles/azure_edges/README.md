# Ansible Role: azure_edges

The `azure_edges` Ansible role is specifically designed for deploying Cisco SD-WAN edge devices, known as Cloud Edge or cEdge, on the Azure cloud platform. It ensures that these instances are deployed following specific configurations and adheres to Azure's best practices.

NOTE: This role is intended to be executed on localhost as it involves making API requests to Azure via Ansible modules from the local machine.

## Role Description

The `azure_edges` role facilitates the deployment of Cisco SD-WAN cEdge instances in Azure. The main tasks include:

- Verifying an active Azure user session.
- Checking hostname constraints to meet Azure specifications.
- Ensuring the presence of all necessary deployment variables.
- Converting resource prefixes to be Azure-compliant.
- Preparing a directory for storing deployment results.
- Confirming that no conflicting instances exist within the designated Azure Virtual Network.
- Creating Azure VM instances for cEdge devices and managing their deployment sequence.
- Recording deployment data and ensuring post-deployment instance accessibility.

## Requirements

- The `cisco.sdwan_deployment` collection installed.
- Ansible 2.16 or higher.
- Ansible Azure modules (`azure.azcollection` collection) installed.
- Azure CLI configured with the necessary permissions to create and manage Azure resources.
- VM images for Cisco Cloud Edge devices should be available in your Azure account.

## Dependencies

- A role named cisco.sdwan_deployment.common`  that includes tasks for verifying Azure dependencies, user sessions, and required variables.
- A role named `azure_network_infrastructure` (if applicable) for managing network resource information.

## Role Variables

### Defaults (`defaults/main.yml`)

Variables with default values that can be overridden by the user:

- `organization_name`: Mandatory field to be defined by the user, used as a prefix for resource naming.
- `az_location`: The Azure location for resource deployment. Must be specified by the user.
- `az_tag_creator`: Tag for identifying resource creator, defaults to the organization name.
- `az_resources_prefix`: Prefix for resources, can be customized by the user.
- `az_resource_group`, `az_virtual_network`, `az_vn_address_prefixes_cidr`, `az_subnets`, `az_network_security_group`: Default configurations for Azure networking resources.
- `az_allowed_subnets`: VPN subnets allowed for Azure public IP connections. Should be defined by the user.
- `azure_key_name`: The Azure key for VM access, to be provided by the user.
- `admin_username`, `admin_password`: Default admin credentials for cEdge instances.
- `vbond_port`, `default_vbond_ip`: Default configurations for vBond.
- `az_cedge_vm_size`: Default Azure VM size for cEdge instances.
- `edge_instances`: List of cEdge instance configurations. If not provided, instances will be created based on PnP Portal information.

### Vars (`vars/main.yml`)

- `results_dir`: Directory to store deployment results.
- `userdata_cedge_path`: Path to the templated userdata configuration for cEdge devices.

### Required Variables

- `organization_name`: The identifier for your organization, used for Azure resource naming.
- `az_location`: The Azure location where resources will be provisioned.
- `az_resource_group`: The name of the Azure resource group for the deployment.
- `az_network_security_group`: The name of the Azure network security group.
- `az_subnets`: The list of subnets to be configured in the Azure Virtual Network.
- `az_cedge_image_offer`: The offer information of the Cisco Edge compute image.
- `az_cedge_image_publisher`: The publisher of the Cisco Edge compute image.
- `az_cedge_image_sku`: The stock-keeping unit (SKU) for the Cisco Edge compute image.
- `az_cedge_image_version`: The version of the Cisco Edge compute image.
- `admin_username`: The admin username for virtual machine access.
- `admin_password`: The admin password for virtual machine access.

## Example Playbook

See [Example playbooks](https://github.com/cisco-open/ansible-collection-sdwan-deployment/tree/main/playbooks).

These playbook reuse configuration files that might be used as example for your configuration

## License

"GPL-3.0-only"

## Author Information

This role was created by Arkadiusz Cichon <acichon@cisco.com>
