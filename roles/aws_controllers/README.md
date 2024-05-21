# Ansible Role: aws_controllers

The `aws_controllers` Ansible role is designed to deploy a Cisco SD-WAN controller topology on AWS cloud infrastructure. It follows the topology outlined in the official Cisco documentation and currently supports the deployment of vManage, vBond, and vSmart instances.

NOTE: Role must be used on localhost - API requests to AWS via boto are done from local machine.

## Role description

The `aws_controllers` role automates the deployment of Cisco SD-WAN controllers (vManage, vBond, and vSmart) in AWS. Key functionalities include:

- Validating AWS dependencies and user sessions.
- Discovering or using provided network infrastructure settings.
- Ensuring all required deployment variables are set.
- Creating EC2 instances for each controller type and managing deployment order.
- Storing deployment data and verifying instance reachability post-setup.

## Requirements

- `cisco.sdwan_deployment` collection installed
- Ansible 2.16 or higher.
- Ansible AWS modules (`amazon.aws` collection) installed.
- Boto3 and Botocore Python libraries installed on the controlling machine to interact with AWS APIs.
- AWS CLI configured with the appropriate permissions to create and manage AWS resources.
- AWS EC2 AMIs for vManage, vBond, and vSmart instances must be available in your AWS account.

## Dependencies

- A role named `cisco.sdwan_deployment.common` that provides tasks for checking AWS boto3 requirements, probing user sessions, and asserting required variables.
- A role named `aws_network_infrastructure` that gathers information about the network resources if not already provided by the user.

## Role Variables

### Defaults (`defaults/main.yml`)

- `organization_name`: Name of the organization deploying the controllers. Must be defined by the user.
- `aws_region`: AWS region where resources will be deployed (default: `us-east-1`).
- `aws_vpc_name`, `aws_security_group_name`: Default naming convention for VPC and security group.
- `aws_tag_creator`: Tag used to mark resources created in AWS.
- `aws_key_name`: AWS SSH key pair name.
- `admin_username`, `admin_password`: Default credentials for controllers.
- `vbond_port`, `default_vbond_ip`: Default port and IP for vBond.
- `aws_vmanage_ami_id`, `aws_vmanage_instance_type`: AMI ID and instance type for vManage.
- `aws_vbond_ami_id`, `aws_vbond_instance_type`: AMI ID and instance type for vBond.
- `aws_vsmart_ami_id`, `aws_vsmart_instance_type`: AMI ID and instance type for vSmart.
- `site_id_vmanage`, `site_id_vbond`, `site_id_vsmart`: Default site IDs for each controller.

### Vars (`vars/main.yml`)

- `results_dir`: Directory to store deployment results.
- `aws_deployed_controllers_data`: File to store data of deployed controllers.
- `userdata_vmanage_path`, `userdata_vbond_path`, `userdata_vsmart_path`: Paths to user data configurations for each controller type.

### Required Variables

The following variables must be set prior to executing the role:

- `organization_name`: The name of your organization, used as a prefix for Azure resources.
- `az_location`: The Azure region where resources will be deployed.
- `az_resource_group`: The name of the Azure resource group for the deployment.
- `az_network_security_group`: The name of the Azure Network Security Group.
- `az_subnets`: A list of subnet definitions for the Azure Virtual Network.
- `admin_username`: Administrator username for the SD-WAN controller instances.
- `admin_password`: Administrator password for the SD-WAN controller instances.

## Example Playbook

See [Example playbooks](https://github.com/cisco-open/ansible-collection-sdwan-deployment/tree/main/playbooks).

## License

"GPL-3.0-only"

## Author Information

This role was created by Arkadiusz Cichon <acichon@cisco.com>
