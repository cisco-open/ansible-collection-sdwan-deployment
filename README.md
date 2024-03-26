# Cisco SD-WAN Deployment on AWS using Ansible

Ansible roles and playbooks for deployment and teardown of Cisco SD-WAN on AWS.

## Table of Contents

- [Overview](#overview)
- [Roadmap](#roadmap)
- [Prerequisites](#prerequisites)
- [Getting Started](#getting-started)
- [Troubleshooting](#troubleshooting)
- [Contact Information](#contact-information)
- [License](#license)
- [Contributing](#contributing)

---

## Overview

This repository includes:

- `aws_network_infrastructure`
- `aws_controllers`
- `aws_edges`
- `aws_teardown`
- `common`

Ansible roles, which can be used to automate the deployment (and teardown) of SD-WAN systems on the AWS cloud.

In order to have more convenient way of handling next onboarding processes, the `aws` role is generating files via:

- `roles/aws_controllers/tasks/generate_deployment_facts.yml` and

- `roles/aws_edges/tasks/generate_deployment_facts.yml`

Path of this output file customizable via `results_dir` `results_path_controllers` and `results_path_edges` variables in input config file.

---

## Roadmap

Current coverage:

- [x] Deployment on AWS
- [x] Deployment of:
  - [x] vManage
  - [x] vBond
  - [x] vSmart
  - [x] cEdge
- [x] Local installation via Ansible Galaxy
- [x] Installation via git repository link

Future Goals:

- [ ] Provide AWX (web-based user interface)
- [ ] Migrate to CiscoDevNet/Cisco Open
- [ ] Share roles via Ansible Galaxy
- [ ] Deployment on Azure
- [ ] Deployment on GCP
- [ ] Support for cluster deployment
- [ ] Enhance cloud-init configuration (complex bringup)

---

## Prerequisites

Before you begin, ensure you have met the following requirements:

- You have installed Ansible
- You have installed Python
- You have an AWS account with the necessary permissions.
- You have access to a Cisco SD-WAN AMIs on AWS.

---

## Getting started

### Using collection in your playbooks

In `requirements.yml` inside your project add:

```yml
- name: git@github.com:cisco-open/ansible-collection-sdwan-deployment.git
  type: git
  version: main
```

Note: If you are not using full ansible installation, you might install also aws.collection by adding:

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

### Python dependencies

The python module dependencies are not installed by ansible-galaxy. They can be manually installed using pip:

```bash
pip install -r requirements.txt
```

### Prepare your configuration

*Note:* Current solution supports topology that consist of vManage, vBond, vSmart and C8000V edge device.

There are configuration files which has been initially filled with values:

- `.playbooks/sdwan_config_20_12.yml`

- `.playbooks/sdwan_config_20_13.yml`

Both files are supplemented by config from `roles/aws_*/vars/example_main.yml` and defaults from `roles/aws_*/defaults/main.yml`

NOTE: You can call the variables file any name, but remember to choose one option:

- include that name in playbook

```yml
- name: Deploy Cisco SD-WAN on AWS
  hosts: localhost
  roles:
    - aws_network_infrastructure
    - aws_controllers
  vars_files:
    - ./playbooks/sdwan_config_20_12.yml
```

- or pass the variables by directly including your configuration file with:

```bash
ansible-playbook playbooks/deploy_controllers_20_12.yml -e "@./playbooks/sdwan_config_20_12.yml"
```

(notice @ that suggest we are reffering to the file)

### Deploying Cisco SD-WAN on AWS

To deploy Cisco SD-WAN on AWS, run the example playbook using roles:

- `aws_network_infrastructure`
- `aws_controllers`
- `aws_edges`

</br>

Current version of this solution assumes that you have used `aws configure` command (AWS CLI) to set your credentials. #TODO other auth methods

We provided example playbooks that you can execute with:

```bash
ansible-playbook playbooks/deploy_controllers_20_12.yml
```

or

```bash
ansible-playbook playbooks/deploy_controllers_20_13.yml
```

and:

```bash
ansible-playbook deploy_edges_20_12.yml
```

For desired changes, please update configuration files.

### Tearing down Cisco SD-WAN on AWS

To teardown the deployed system, run the example playbook using the `aws_teardown` role.

```bash
ansible-playbook ./playbooks/teardown_20_12.yml

or

ansible-playbook ./playbooks/teardown_20_13.yml
```

If you want to teardown only specific ec2 instances (with their EiPs and NICs associated):

```bash
ansible-playbook ./playbooks/teardown_20_12.yml -e "@instances_to_teardown.yml"
```

Where `instances_to_teardown.yml` is path to file with definition:

```yml
teardown_specific_instances:
  - "acich-ansible-cedge-111"
  - "acich-ansible-cedge-222"
```

---

## Troubleshooting

### 1. Consol connectivity works, but cannot reach with SSH or ICMP

If your instances are up and running, and you can log to them via ec2 console, please verify that your ip address
is "allow-listed". See `aws_allowed_subnets` in `roles/aws_controllers/defaults/main.yml` to verify.

### 2. Services status

If vManage is not starting NMS service:

- check if your disk /opt/data is more than 20% free. Otherwise that case shutdown application as well
- remember to use at least `c5.9xlarge` instance type for vManage in AWS

---

## Compatibility

We are supporting python3.8 currently.
Ansible-lint is 6.11

---

## Contact Information

For any questions or concerns, please open an issue on this repository.

## License

See [LICENSE](./LICENSE) file.

## Contributing

See [Contributing](./docs/CONTRIBUTING.md) file.
