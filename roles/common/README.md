# Ansible Role: cisco.sdwan_deployment.common

The `cisco.sdwan_deployment.common` Ansible role is a utility role that provides common tasks used by other roles within the `cisco.sdwan_deployment` collection. These tasks include checking user sessions, probing for existing instances, ensuring necessary requirements are met, and preparing for SD-WAN deployment on cloud platforms like AWS and Azure.

## Role Description

The `common` role includes the following key tasks:

- Verifying that the necessary `boto3` library is installed for AWS deployments.
- Probing the current user session for AWS and Azure to ensure that API calls can be made successfully.
- Checking for existing instances on AWS and Azure to prevent resource conflicts.
- Generating deployment facts for Cisco SD-WAN controllers and edge devices.
- Waiting for SSH readiness to ensure that instances are accessible for further configuration.
- Asserting that all required variables for different stages of SD-WAN deployment are present.

## Requirements

- The `cisco.sdwan_deployment` collection installed.
- Ansible 2.16 or higher.
- For AWS deployments: Python `boto3` library and AWS CLI configured with necessary permissions.
- For Azure deployments: Ansible Azure modules (`azure.azcollection` collection) installed and Azure CLI configured with necessary permissions.

## Dependencies

This role does not have dependencies on other roles but is a dependency for other roles within the `cisco.sdwan_deployment` collection.

## Role Variables

The `common` role does not directly define variables but instead checks for variables required by other roles. Examples of such variables include cloud provider credentials, SD-WAN instance specifications, and deployment settings which should be provided by the user or defined in other roles that include the `common` tasks.

## License

"GPL-3.0-only"

## Author Information

This role is provided as part of the `cisco.sdwan_deployment` collection, role was created by Arkadiusz Cichon <acichon@cisco.com>
