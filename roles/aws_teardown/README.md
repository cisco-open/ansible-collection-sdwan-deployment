# AWS Teardown Ansible Role Documentation

## Overview

The `aws_teardown` role is designed to safely decommission and remove AWS resources that were previously deployed, ensuring that all associated components are properly cleaned up.

NOTE: This role should be executed on localhost as it performs API requests to AWS via boto from the local machine.

## Role Description

This role provides a systematic approach to tearing down AWS resources, with a strong emphasis on safety and confirmation. Key functionalities include:

- Prompting the user for confirmation before proceeding with the teardown to avoid accidental deletions.
- Verifying that the user's AWS session is active to ensure API call capability.
- Retrieving details of the VPC created by the user and confirming its existence.
- Gathering information about all subnets associated with the VPC.
- Conditionally terminating specific EC2 instances or removing all resources within the VPC, including subnets, route tables, internet gateways, and the VPC itself.

## Requirements

- `cisco.sdwan_deployment` collection installed.
- Ansible 2.16 or higher.
- Ansible AWS modules (`amazon.aws` collection) installed.
- Boto3 and Botocore Python libraries installed on the controlling machine to interact with AWS APIs.
- AWS CLI configured with the appropriate permissions to delete AWS resources.

## Dependencies

- A role named cisco.sdwan_deployment.common`  that includes tasks for probing the user's AWS session.

## Role Variables

### Defaults (`defaults/main.yml`)

- `organization_name`: Name of the organization. Must be defined by the user.
- `teardown_resources_data_path`: Path where the teardown data JSON file will be stored.
- `teardown_only_instances`: Boolean value to indicate if only EC2 instances should be torn down.
- `teardown_specific_instances`: Boolean value to indicate if specific EC2 instances should be torn down.
- `aws_region`: AWS region where resources were deployed (default: `us-east-1`).
- `aws_availibility_zone`: AWS availability zone used for resource deployment (default: `us-east-1a`).
- `aws_vpc_name`, `aws_security_group_name`: Names for the VPC and security group to be removed.

## Example Playbook

See [Example playbooks](https://github.com/cisco-open/ansible-collection-sdwan-deployment/tree/main/playbooks).

These playbook reuse configuration files that might be used as example for your configuration

## License

"GPL-3.0-only"

## Author Information

This role was created by Arkadiusz Cichon <acichon@cisco.com>
