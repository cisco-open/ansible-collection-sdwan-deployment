# AWS Edges Ansible Role Documentation

## Overview

The `aws_edges` role, part of the `cisco.sdwan_deployment` collection, facilitates the deployment of Cisco SD-WAN edge devices (cEdges) within an AWS environment.

NOTE: Role must be used on localhost - API requests to AWS via boto are done from local machine.

## Role description

The `aws_edges` role is an essential component of the `cisco.sdwan_deployment` collection and focuses on the automated deployment of Cisco SD-WAN edge devices (cEdges) in the AWS cloud. Key functionalities include:

- Validating dependencies on boto3 and botocore for AWS interactions.
- Confirming the presence of an active AWS user session.
- Discovering or incorporating provided network configurations such as VPC, security groups, and subnets.
- Asserting the availability of all necessary variables for the deployment of edge devices.
- Deploying EC2 instances for cEdge devices and configuring them according to specified parameters.
- Organizing deployment results and confirming the operational status of the instances through SSH reachability checks.

## Requirements

- `cisco.sdwan_deployment` collection installed
- Ansible 2.16 or higher.
- Ansible AWS modules (`amazon.aws` collection) installed.
- Boto3 and Botocore Python libraries installed on the controlling machine to interact with AWS APIs.
- AWS CLI configured with the appropriate permissions to create and manage AWS resources.
- AWS EC2 AMIs for vManage, vBond, and vSmart instances must be available in your AWS account.

## Dependencies

- A role named cisco.sdwan_deployment.common`  that provides tasks for AWS boto3 requirements, user session checks, variable verifications, instance checks, and deployment fact gathering.
- Prepared network infrastructure used to deploy instances to (VPC, subnets etc.)

## Role Variables

### Defaults (`defaults/main.yml`)

- `organization_name`: Name of the organization. Must be defined by the user.
- `aws_region`: AWS region where resources will be deployed (default: `us-east-1`).
- `aws_vpc_name`, `aws_security_group_name`: Defaults for naming VPC and security group resources.
- `aws_tag_creator`: Tag for identifying the creator of AWS resources.
- `aws_key_name`: AWS SSH key pair name for EC2 instances.
- `admin_username`, `admin_password`: Default admin credentials for edge devices.
- `vbond_port`, `default_vbond_ip`: Default vBond communication settings.
- `aws_cedge_ami_id`, `aws_cedge_instance_type`: AMI ID and instance type for cEdge devices.
- `edge_instances`: List of edge device instances to be deployed.

### Vars (`vars/main.yml`)

- `results_dir`: Directory where deployment results will be stored.
- `aws_deployed_edges_data`: File to store data of deployed edge devices.
- `userdata_cedge_path`: Path to the user data configuration for cEdge devices.

### Required variables

The following variables must be set prior to executing the role:

- `organization_name`: Identifier for your organization, used for naming AWS resources.
- `aws_region`: AWS region to host the resources.
- `aws_availibility_zone`: Specific AWS availability zone within the selected region.
- `admin_password`: Password for administrative access to controller instances.
- `aws_vpc_config`: Configuration details for the AWS VPC.
- `aws_security_group_config`: Settings for the AWS security group.
- `aws_subnets_config`: Specifications for the AWS subnets.
- `aws_cedge_ami_id`: AMI ID for the Cisco Edge compute instances.

## Example Playbook

See [Example playbooks](https://github.com/cisco-open/ansible-collection-sdwan-deployment/tree/main/playbooks).

## License

"GPL-3.0-only"

## Author Information

This role was created by Arkadiusz Cichon <acichon@cisco.com>
