_# Ansible Role: azure_device_params

The `azure_device_params` Ansible role reads params from cEdge devices deployed on Azure, so that they can be used through other roles.

## Role Description

The `azure_device_params` role generates deployment facts for already deployed cEdge devices. For each cEdge deployment facts contain information about its:
- `hostname`
- `admin_username`
- `admin_password`
- `mgmt_public_ip`
- `transport_public_ip`
- `service_interfaces`
Additionally the role sets the `manager_authentication` variable, which can be used for logging to vManage in other roles.

## Requirements

- The `cisco.sdwan_deployment` collection installed.
- Ansible 2.16 or higher.
- Ansible Azure modules (`azure.azcollection` collection) installed.
- Azure CLI configured with the necessary permissions to manage Azure resources.

## Dependencies

There are no external role dependencies. Only `cisco.sdwan_deployment` collection is required.

### Required Variables

- `admin_password`: The admin password for virtual machine access.
- `az_resource_group`: The name of the Azure resource group for the deployment.

## Example Playbook

Including an example of how to use your role (for instance, with variables passed in as parameters):

```yaml
- name: Read deployed cEdge parameters
  hosts: localhost
  gather_facts: false
  vars:
    az_resource_group: "resource-group"
    admin_password: "password"  # pragma: allowlist secret
  roles:
    - cisco.sdwan_deployment.azure_device_params
```

## License

"GPL-3.0-only"

## Author Information

This role was created by Przemyslaw Susko <sprzemys@cisco.com>_
