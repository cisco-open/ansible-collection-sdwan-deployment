# Ansible Role: azure_teardown

The `azure_teardown` Ansible role is designed to dismantle the Azure cloud infrastructure associated with a particular organization's deployment. This role primarily focuses on removing Azure resource groups and their contained resources, which is an essential step for clean-up operations or decommissioning environments.

NOTE: This role is to be executed on localhost as it requires direct API interactions with Azure services from the control machine.

## Role Description

The `azure_teardown` role performs the necessary actions to de-provision and remove Azure resources created during the SD-WAN deployment or other Azure-based projects. Its primary functions are:

- Verifying that an active user session with Azure exists.
- Standardizing the Azure resource prefix to comply with Azure naming restrictions.
- Removing the specified Azure Resource Group and its associated resources.
- Optionally waiting for the entire teardown process to complete before exiting the playbook.

## Requirements

- The `cisco.sdwan_deployment` collection installed.
- Ansible 2.16 or higher.
- Ansible Azure modules (`azure.azcollection` collection) installed.
- Azure CLI configured with the necessary permissions to delete Azure resources.

## Dependencies

- A role named `cisco.sdwan_deployment.common` that includes tasks for verifying Azure dependencies and user sessions.

## Role Variables

### Defaults (`defaults/main.yml`)

Variables with default values that may need to be overridden by the user:

- `organization_name`: The name of the organization associated with the resources being torn down. It must be defined by the user.
- `wait_for_teardown`: Boolean flag to indicate whether the playbook should wait for the teardown process to complete (default: `true`).
- `az_location`: The Azure location where the network resources are deployed. Must be specified by the user if needed for teardown.
- `az_tag_creator`: Tag used to identify the creator of the resources, defaults to the organization name.
- `az_resources_prefix`: Prefix for the Azure resources, defaulting to the organization name but customizable by the user.
- `az_resource_group`: Default name for the Azure resource group to be removed.

## Example Playbook

See [Example playbooks](https://github.com/cisco-open/ansible-collection-sdwan-deployment/tree/main/playbooks).

These playbook reuse configuration files that might be used as example for your configuration.

## License

"GPL-3.0-only"

## Author Information

This role was created by Arkadiusz Cichon <acichon@cisco.com>
