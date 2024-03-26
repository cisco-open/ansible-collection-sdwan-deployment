# How to contribute

Thank you for investing your time in contributing to our project!

---

## Issues

### Solve an issue

See [existing issues](https://sdwan-git.cisco.com/sdwan-tools/cisco.sdwan_deployment/issues) and feel free to work on any.

### Create a new issue

Firstly [search if an issue already exists](hhttps://sdwan-git.cisco.com/sdwan-tools/cisco.sdwan_deployment/issues).

If issue related to your problem/feature request doesn't exists, create new issue.
There are 3 issue types:

- Bug report
- Feature Request
- Report a security vulnerability

Select one from [issue form](https://sdwan-git.cisco.com/sdwan-tools/cisco.sdwan_deployment/issues/new/choose).

### Create PR

When you're finished with the changes, create a pull request, also known as a PR.

---

## Development

1. Clone this repository.
2. Install the required requirements:

```bash
pip install -r requirements.txt
```

3. Install required amazon.aws collection:

```bash
ansible-galaxy install -r requirements.yml
```

You can reuse existing playbooks to test your code changes.
