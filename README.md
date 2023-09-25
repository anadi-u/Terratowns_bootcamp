# Terraform Beginner Bootcamp 2023

We are going to use Semantic Versioning for tagging in this project.
[semver.org](https://semver.org/)

The format would look like this; **MAJOR.MINOR.PATCH**, eg. `1.0.0`

- **MAJOR** version when you make incompatible API changes
- **MINOR** version when you add functionality in a backward compatible manner
- **PATCH** version when you make backward compatible bug fixes

## Installing Terrafrom CLI

-Installation instructions have been changed due to `Warning:apt-key is deprecated` issue.

- We replaced `init` with `before` 
[Reference Link](https://www.gitpod.io/docs/configure/workspaces/tasks)

-[Official source of Terraform CLI installation](https://developer.hashicorp.com/terraform/tutorials/aws-get-started/install-cli)

-[More info on deprecated apt-key](https://www.techrepublic.com/article/how-to-fix-the-apt-key-deprecated-warning-in-ubuntu/)


## Refactoring bash scripts

-We also created `install_terraform_cli.sh` bash script with required instructions and executed it in the `gitpod.yml` file.
This would automate the process of installing the Terraform CLI on the Gitpod instance.

- The bash script is located in [install_terraform_cli](/bin/install_terraform_cli.sh)

-A quick note: `-y` flag can be used to automate the apt-update/install commands (Eliminating the need of using enter key for further processing of the command)

## Linux commands and issues

- For checking the distro, we used:
```
$ cat /etc/os-release
PRETTY_NAME="Ubuntu 22.04.3 LTS"
NAME="Ubuntu"
VERSION_ID="22.04"
VERSION="22.04.3 LTS (Jammy Jellyfish)"
VERSION_CODENAME=jammy
ID=ubuntu
ID_LIKE=debian
HOME_URL="https://www.ubuntu.com/"
SUPPORT_URL="https://help.ubuntu.com/"
BUG_REPORT_URL="https://bugs.launchpad.net/ubuntu/"
PRIVACY_POLICY_URL="https://www.ubuntu.com/legal/terms-and-policies/privacy-policy"
UBUNTU_CODENAME=jammy
```

- If you do not include `#!/bin/bash` in the script, you would receive : `bad interpreter: No such file or directory` error, while trying to execute the script.



