# Ansible Role: azure_controllers

The `azure_controllers` role is designed to deploy SD-WAN controller instances, specifically vManage, vBond, and vSmart, on the Azure cloud platform. It ensures that instances are deployed according to specific configurations.

NOTE: This role should be executed on localhost as it performs API requests to Azure via the Ansible Azure modules from the local machine.

## Role Description

The `azure_controllers` role automates the deployment of Cisco SD-WAN controllers (vManage, vBond, and vSmart) in Azure. Key functionalities include:

- Verifying the active Azure user session.
- Asserting that all required variables for Azure controller deployment are set.
- Validating the hostname length for each instance to comply with Azure naming constraints.
- Ensuring that Azure resource prefixes contain hyphens instead of underscores.
- Preparing a directory to store results and deployment data.
- Checking for existing instances in the specified Azure Virtual Network (VN) to avoid conflicts.
- Defining the deployment facts for Ansible to consume.
- Creating Azure VMs for vBond, vSmart, and vManage instances.
- Extracting deployment facts post-deployment.
- Checking the reachability of the vManage instance via SSH to confirm deployment success.

## Requirements

- `cisco.sdwan_deployment` collection installed
- Ansible 2.16 or higher.
- Azure CLI installed and configured with appropriate permissions.
- Ansible Azure modules (`azure.azcollection`) installed.
- VM images for Cisco controller devices should be available in your Azure account.

## Dependencies

- A role named cisco.sdwan_deployment.common`  that includes tasks for probing the Azure user session, verifying required variables, and checking for existing instances.
- A role named `azure_network_infrastructure` (if applicable) for managing network resource information.

## Role Variables

### Defaults (`defaults/main.yml`)

- `organization_name`: User-defined organization name, used as a prefix for Azure resources.
- `az_location`: Azure location where resources will be deployed. Must be defined by the user.
- `az_resources_prefix`: Prefix for Azure resources, defaults to the organization name.
- `az_resource_group`: Name of the Azure resource group.
- `az_virtual_network`: Name of the Azure Virtual Network.
- `az_vn_address_prefixes_cidr`: CIDR block for the Azure Virtual Network.
- `az_subnets`: Definitions for Azure subnets within the Virtual Network.
- `az_network_security_group`: Name of the Azure Network Security Group.
- `az_allowed_subnets`: VPN subnets allowed to connect to Azure public IPs.
- `azure_key_name`: Name of the Azure key for VM access.
- `admin_username`: Default admin username for deployed VMs.
- `admin_password`: Default admin password for deployed VMs.
- `az_vmanage_vm_size`, `az_vbond_vm_size`, `az_vsmart_vm_size`: Azure VM sizes for vManage, vBond, and vSmart instances.
- `site_id_vmanage`, `site_id_vbond`, `site_id_vsmart`: Default site IDs for vManage, vBond, and vSmart instances.
- `vmanage_instances`, `vbond_instances`, `vsmart_instances`: Lists for instance configurations.

### Vars (`vars/main.yml`)

- `results_dir`: Directory where deployment results are stored.
- `userdata_vmanage_path`, `userdata_vbond_path`, `userdata_vsmart_path`: Paths to templated userdata configurations for each controller type.

### Required Variables

- `organization_name`: Your organization's name for resource naming in Azure.
- `az_location`: The Azure region for resource deployment.
- `az_resource_group`: The Azure resource group name for organizing resources.
- `az_network_security_group`: The name of the Azure Network Security Group.
- `az_subnets`: Definitions of Azure subnets within the Virtual Network.
- `admin_username`: Admin username for the deployed VMs.
- `admin_password`: Admin password for the deployed VMs.

## Example Playbook

See [Example playbooks](https://github.com/cisco-open/ansible-collection-sdwan-deployment/tree/main/playbooks).

These playbook reuse configuration files that might be used as example for your configuration.

## License

"GPL-3.0-only"

## Author Information

This role was created by Arkadiusz Cichon <acichon@cisco.com>
