# Ansible Role: template_cloudinit

The `template_cloudinit` Ansible role is created to generate cloud-init configuration files for different types of Cisco SD-WAN controllers and edge devices. The role supports cloud providers such as AWS and Azure and prepares userdata scripts that are used to bootstrap these instances upon creation.

## Role Description

The `template_cloudinit` role executes the following tasks:

- Prompts the user for the cloud provider or reads it from the configuration.
- Validates the cloud provider input.
- Asserts the presence of all required variables for cloudinit generation.
- Prepares a directory to store the generated cloudinit files.
- Generates cloudinit templates for vBond, vManage, vSmart, and cEdge instances.
- Displays the location and list of generated cloudinit files to the user.

## Requirements

- The `cisco.sdwan_deployment` collection installed.
- Ansible 2.16 or higher.
- Jinja2 templates for cloud-init userdata scripts corresponding to each type of device (vBond, vManage, vSmart, cEdge).

## Dependencies

- A role named `common` that contains tasks for verifying required variables.

## Role Variables

### Defaults (`defaults/main.yml`)

Variables with default values that the user may need to override:

- `results_dir`: Directory to store generated cloudinit files.
- `userdata_vmanage_path`, `userdata_vbond_path`, `userdata_vsmart_path`: Paths to templated userdata configurations for respective SD-WAN controllers.
- `admin_username`, `admin_password`: Default admin credentials used in userdata scripts.
- `vbond_port`, `default_vbond_ip`: Default configurations for vBond.
- `vbond_transport_private_ip`, `vbond_transport_public_ip`: IPs for vBond, to be defined by the user if static IPs are used.
- `site_id_vmanage`, `vmanage_instances`: Site ID and list of vManage instances.
- `site_id_vbond`, `vbond_instances`: Site ID and list of vBond instances.
- `site_id_vsmart`, `vsmart_instances`: Site ID and list of vSmart instances.
- `edge_instances`: List of cEdge instances.

### Required Variables

- `organization_name`: The name of your organization, referenced in the cloud init configuration.
- `admin_username`: The administrative username for initial server setup.
- `admin_password`: The administrative password for initial server setup.
- `vbond_transport_private_ip`: The private IP address for vBond's transport interface.
- `vbond_transport_public_ip`: The public IP address for vBond's transport interface.

## Example Playbook

See [Example playbooks](https://github.com/cisco-open/ansible-collection-sdwan-deployment/tree/main/playbooks).

These playbook reuse configuration files that might be used as example for your configuration.

## License

"GPL-3.0-only"

## Author Information

This role was created by Arkadiusz Cichon <acichon@cisco.com>
