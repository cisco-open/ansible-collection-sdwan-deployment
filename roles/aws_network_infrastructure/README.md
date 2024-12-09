# AWS Network Infrastructure Ansible Role Documentation

## Overview

The role provided here is designed to establish foundational network infrastructure within an AWS environment as a prerequisite for deploying other resources.

NOTE: This role should be executed on localhost as it performs API requests to AWS via boto from the local machine.

## Role Description

This role focuses on creating and configuring the necessary network components in AWS, such as VPCs, subnets, security groups, and internet gateways. The key functionalities include:

- Validating boto3 and botocore dependencies, ensuring that AWS SDKs are available for Python.
- Confirming the user's AWS session is active for making API requests.
- Asserting that all required variables are provided to configure the network infrastructure.
- Preparing a results directory to store infrastructure deployment information.
- Creating and managing AWS network infrastructure elements using the provided configurations.

## Requirements

- `cisco.sdwan_deployment` collection installed.
- Ansible 2.16 or higher.
- Ansible AWS modules (`amazon.aws` collection) installed.
- Boto3 and Botocore Python libraries installed on the controlling machine to interact with AWS APIs.
- AWS CLI configured with the appropriate permissions to create and manage AWS resources.

## Dependencies

- A role named cisco.sdwan_deployment.common`  that includes tasks for checking AWS boto3 requirements, probing the user's AWS session, and verifying required variables.

## Role Variables

### Defaults (`defaults/main.yml`)

- `aws_allowed_subnets`: VPN subnets allowed to connect to AWS Elastic IPs.
- `organization_name`: Name of the organization. Must be defined by the user.
- `aws_vpc_name`, `aws_vpc_cidr`: Defaults for naming and CIDR of the VPC.
- `aws_igw_name`: Name for the AWS Internet Gateway.
- `aws_subnets`: List of subnet configurations for the VPC.
- `aws_route_table_name`: Name for the VPC's route table.
- `aws_security_group_name`: Name for the security group.
- `aws_vpn_name`, `aws_eip_name`, `aws_nacl_name`: Names for VPN, Elastic IP, and network ACL.

### Vars (`vars/main.yml`)

- `results_dir`: Directory where deployment results will be stored.
- `aws_deployed_network_data`: File to store data of deployed network components.

### Required Variables

Before running the role, define the following variables:

- `organization_name`: The name of your organization, influencing AWS resource naming.
- `aws_region`: The AWS region for deploying resources.
- `aws_availibility_zone`: The desired AWS availability zone within the region.
- `aws_allowed_subnets`: List of subnets allowed to interact with the AWS resources.

## Example Playbook

See [Example playbooks](https://github.com/cisco-en-programmability/ansible-collection-sdwan-deployment/tree/main/playbooks).

## License

"GPL-3.0-only"

## Author Information

This role was created by Arkadiusz Cichon <acichon@cisco.com>
