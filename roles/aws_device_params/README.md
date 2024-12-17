# Ansible Role: aws_device_params

The `aws_device_params` Ansible role reads params from cEdge devices deployed on AWS, so that they can be used through other roles.

## Role Description

The `aws_device_params` role generates deployment facts for already deployed cEdge devices. For each cEdge deployment facts contain information about its:
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
- Ansible AWS modules (`amazon.aws` collection) installed.
- AWS CLI configured with the appropriate permissions to create and manage AWS resources.

## Dependencies

There are no external role dependencies. Only `cisco.sdwan_deployment` collection is required.

### Required Variables

- `aws_tag_creator`: Tag for identifying the creator of AWS resources.
- `aws_region`: AWS region to host the resources.
- `admin_password`: The admin password for virtual machine access.

## Example Playbook

Including an example of how to use your role (for instance, with variables passed in as parameters):

```yaml
- name: Read deployed cEdge parameters
  hosts: localhost
  gather_facts: false
  vars:
    aws_region: "us-east-1"
    aws_tag_creator: "tag-creator"
    admin_password: "password"  # pragma: allowlist secret
  roles:
    - cisco.sdwan_deployment.aws_device_params
```

## License

"GPL-3.0-only"

## Author Information

This role was created by Przemyslaw Susko <sprzemys@cisco.com>
