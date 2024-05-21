# Cisco SD-WAN Deployment on AWS and Azure using Ansible

Ansible roles and playbooks for deployment and teardown of Cisco SD-WAN on AWS and Azure.

## Table of Contents

- [Overview](#overview)
- [Roadmap](#roadmap)
- [Requirements](#requirements)
- [Installing this collection](#installing-this-collection)
- [Using this collection](#using-this-collection)
- [Troubleshooting](#troubleshooting)
- [Useful Links](#useful-links)
- [Contact Information](#contact-information)
- [License](#license)
- [Contributing](#contributing)
- [Code of Conduct](#code-of-conduct)
- [Releasing, Versioning and Deprecation](#releasing-versioning-and-deprecation)

---

## Overview

This repository includes:

- `aws_network_infrastructure`
- `aws_controllers`
- `aws_edges`
- `aws_teardown`
- `common`
- `azure_controllers`
- `azure_edges`
- `azure_teardown`
- `azure_controllers`
- `template_cloudinit`

Ansible roles, which can be used to automate the deployment (and teardown) of SD-WAN systems on the AWS cloud.

In order to have more convenient way of handling next onboarding processes, the `aws` and `azure` roles are generating files via:

- `roles/common/tasks/generate_deployment_facts_controllers.yml` and

- `roles/common/tasks/generate_deployment_facts_edges.yml`

Path of this output file customizable via `results_dir` `results_path_controllers` and `results_path_edges` variables in input config file.

---

## Roadmap

Current coverage:

- [x] Deployment on AWS
- [x] Deployment on Azure
- [x] Deployment of:
  - [x] vManage
  - [x] vBond
  - [x] vSmart
  - [x] cEdge
- [x] Local installation via Ansible Galaxy
- [x] Installation via git repository link
- [x] Migration to CiscoDevNet/Cisco Open
- [x] Separate role for cloudinit templating

Future Goals:

- [ ] Provide AWX (web-based user interface)
- [ ] Share roles via Ansible Galaxy
- [ ] Deployment on GCP
- [ ] Support for cluster deployment
- [ ] Enhance cloud-init configuration (complex bringup)

---

## Requirements

This collection is based on `ansible-core==2.16.6`, see [ansible-core-support-matrix](https://docs.ansible.com/ansible/latest/reference_appendices/release_and_maintenance.html#ansible-core-support-matrix).

Before you begin, ensure you have met the following requirements:

- You have installed Python 3.10 - 3.12
- You have an AWS or Azure account with the necessary permissions
- You have access to a Cisco SD-WAN AMIs on AWS or images on Azure

### Python dependencies

The python module dependencies are not installed by ansible-galaxy. They can be manually installed using pip:

```bash
pip install -r requirements.txt
```

---

## Installing this collection

### Using `requirements.yml`

In `requirements.yml` inside your project add:

```yml
- name: git@github.com:cisco-open/ansible-collection-sdwan-deployment.git
  type: git
  version: main
```

Note: If you are not using full ansible installation, you might install also `aws.collection` and `azure.azcollection` by adding:

```yml
  - name: amazon.aws
    version: 6.5.0
  - name: azure.azcollection
    version: 1.19.0
```

to `requirements.yml` inside your project.

At the end always run:

```bash
ansible-galaxy install -r requirements.yml
```

## Using this collection

### Prepare your configuration

*Note:* Current solution supports topology that consist of vManage, vBond, vSmart and C8000V edge device.

There are configuration files which has been initially filled with values:

- `.playbooks/aws_sdwan_config.yml`
- `.playbooks/azure_sdwan_config.yml`

Both files are supplemented by config defaults from all roles.

NOTE: You can call the variables file any name, but remember to choose one option:

- include that name in playbook

```yml
- name: Deploy Cisco SD-WAN on AWS
  hosts: localhost
  roles:
    - aws_network_infrastructure
    - aws_controllers
  vars_files:
    - ./playbooks/aws_sdwan_config.yml
```

- or pass the variables by directly including your configuration file with:

```bash
ansible-playbook playbooks/aws_deploy_controllers.yml -e "@./playbooks/aws_sdwan_config.yml"
```

(notice @ that suggest we are reffering to the file)

### Deploying Cisco SD-WAN

To deploy Cisco SD-WAN on AWS or Azure, run the example playbook using roles:

For AWS:

- `aws_network_infrastructure`
- `aws_controllers`
- `aws_edges`

For Azure:

- `azure_network_infrastructure`
- `azure_controllers`
- `azure_edges`

</br>

Current version of this solution assumes that users will authenticate with their cloud providers in order to run ansible playbooks. See [Useful Links](#useful-links).

We provided example playbooks that you can execute with:

```bash
ansible-playbook playbooks/aws_deploy_controllers.yml
ansible-playbook playbooks/aws_deploy_edges.yml
```

or

```bash
ansible-playbook playbooks/azure_deploy_controllers.yml
ansible-playbook playbooks/azure_deploy_edges.yml
```

For desired changes, please update configuration files.

### Tearing down Cisco SD-WAN on AWS

To teardown the deployed system, run the example playbook using the `aws_teardown` role or `azure_teardown`.

```bash
ansible-playbook ./playbooks/aws_teardown.yml

or

ansible-playbook ./playbooks/azure_teardown.yml
```

If you want to teardown only specific ec2 instances (with their EiPs and NICs associated):

```bash
ansible-playbook ./playbooks/aws_teardown.yml -e "@instances_to_teardown.yml"
```

Where `instances_to_teardown.yml` is path to file with definition:

```yml
teardown_specific_instances:
  - "acich-ansible-cedge-111"
  - "acich-ansible-cedge-222"
```

### Generating cloud-init configuration

Role `template_cloudinit` provide tasks that can generate `cloudinit` (also known as `userdata`) configuration, without deployment of any machines.
Examples usage of `template_cloudinit` role can be taken from `playbooks/template_cloudinit.yml`. Note, that in this example playbook, configuration file
is used from `playbooks/template_cloudinit.yml`.

---

## Troubleshooting

### 1. Consol connectivity works, but cannot reach with SSH or ICMP

If your instances are up and running, and you can log to them via ec2 console, please verify that your ip address
is "allow-listed". See `aws_allowed_subnets` in `roles/aws_controllers/defaults/main.yml` to verify.

### 2. Services status

If vManage is not starting NMS service:

- check if your disk /opt/data is more than 20% free. Otherwise that case shutdown application as well
- remember to make sure the sdwan manager and other sdwan virtual machines are right sized for your deployment needs - cisco's server recommendations are available here: [server-requirements](https://www.cisco.com/c/en/us/td/docs/routers/sdwan/release/notes/compatibility-and-server-recommendations/server-requirements.html)

---

## Compatibility

Note that azure collection python requirements include package `uamqp` which can generate wheel issues.
For MacOS you migth install cmake: `brew install cmake` and: `pip install cmake`.
Then install working `uamqp` package (which is below `v1.6.9`) with: `pip install uamqp==1.6.8`.

---

## Useful links

### AWS CLI

- [Installing AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-install.html)
- [Configuring the AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-configure.html)

### AWS Authentication

- [Understanding and Getting Your Security Credentials](https://docs.aws.amazon.com/general/latest/gr/aws-sec-cred-types.html)
- [Configuring AWS Credentials](https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-files.html)

### Azure Authentication

- [Authenticating with Azure](https://docs.ansible.com/ansible/latest/scenario_guides/guide_azure.html#authenticating-with-azure)

---

## Contact Information

For any questions or concerns, please open an issue on this repository.

## License

See [LICENSE](./LICENSE) file.

## Contributing

See [Contributing](./docs/CONTRIBUTING.md) file.

## Code of Conduct

See [Code of Conduct](./docs/CODE_OF_CONDUCT.md) file.

## Releasing, Versioning and Deprecation

This collection follows Semantic Versioning. More details on versioning can be found in [Understanding collection versioning](https://docs.ansible.com/ansible/latest/dev_guide/developing_collections_distributing.html#understanding-collection-versioning).
