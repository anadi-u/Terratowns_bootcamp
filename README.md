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


### Working with env var in Bash

-Environmental variables(env var) are variables that are defined for the current shell and are inherited by any child shells or processes

-To list out all env vars, we can use 'env' command
-To filter out the env vars, we can use grep, Eg: 

```sh
$env | grep terraform-beginner-bootcamp

GITPOD_REPO_ROOT=/workspace/terraform-beginner-bootcamp-2023
PWD=/workspace/terraform-beginner-bootcamp-2023
THEIA_WORKSPACE_ROOT=/workspace/terraform-beginner-bootcamp-2023
GITPOD_WORKSPACE_CONTEXT_URL=https://github.com/anadi-u/terraform-beginner-bootcamp-2023/tree/5-project-root-env-var
GITPOD_REPO_ROOTS=/workspace/terraform-beginner-bootcamp-2023
GITPOD_WORKSPACE_CONTEXT={"ref":"5-project-root-env-var","refType":"branch","isFile":false,"path":"","title":"anadi-u/terraform-beginner-bootcamp-2023 - 5-project-root-env-var","revision":"0deb78e837f969c562c42f5143ba39b18c96885a","repository":{"cloneUrl":"https://github.com/anadi-u/terraform-beginner-bootcamp-2023.git","host":"github.com","defaultBranch":"main","name":"terraform-beginner-bootcamp-2023","owner":"anadi-u","private":false},"normalizedContextURL":"https://github.com/anadi-u/terraform-beginner-bootcamp-2023/tree/5-project-root-env-var","checkoutLocation":"terraform-beginner-bootcamp-2023"}
```

- We can set a temporary env var by:
```sh
HELLO = 'world' ./bin/print_message
```

-An env var can also be defined within a script. Eg:

```sh
#!/bin/bash
MESSAGE='Hello'
echo $MESSAGE
```
-We can export the env var with executing the script in the terminal. Eg: `export MESSAGE=Hello world`

-To unset an env var, `unset MESSAGE`

### AWS CLI installation

- AWS CLI is installed via `./bin/install_aws_cli` script
- [Getting started with AWS CLI installation](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html)

- We created an IAM user in AWS in order to use its access keys during AWS CLI installation.

- We can check if AWS credentials are set correctly by using the following command:
```sh
aws sts get-caller-identity
```

- If its successful, you should get an output in JSON format:

```json
> aws sts get-caller-identity
{
    "UserId": "BYTDAUIA3YDL2N4WX7876JJ",
    "Account": "123456789012",
    "Arn": "arn:aws:iam::123456789012:user/terraform_beginner_bootcamper"
}
```

- [Setting AWS CLI env vars](https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-envvars.html)

- <font color="red">**Never**</font> put your credentials in the cloud dev environments as hackers can scan the repo and get access to them.


- We want to remove the awscliv2.zip along with the aws directory whenever we execute the `install_aws_cli` script. This will not ask us to replace the existing AWS installtion when we run the installtion script.

- Before adding remove commands from the script, this prompt would come up asking for replacing the existing installation:

```sh
replace aws/THIRD_PARTY_LICENSES? [y]es, [n]o, [A]ll, [N]one, [r]ename:
```
- After adding the rm command (remove), we would not get this prompt on running the script again.

- **Quick note:** rm command with `-rf` is used to delete the directories with files inside them. `rmdir` command can only be used to delete directories which are empty.

- The `-r` flag in the `rm` command stands for "recursive." When you use `rm` with the `-r` flag, it tells the command to remove directories and their contents recursively.

## Terraform Basics

### Terraform Registry
Terraform sources their providers and modules from the Terrform registry which is located at [registry.terraform.io](registry.terraform.io)

- **Providers:**
 Plugins that define and manage the resources for a specific infrastructure platform or service. Providers serve as the interface between Terraform and the underlying infrastructure, allowing you to create, update, and delete resources on the supported platforms.
 -Example of a provider: [Random Provider](https://registry.terraform.io/providers/hashicorp/random/latest)

- **Modules:**
  Enable you to break down your Terraform configuration into smaller, more manageable pieces, making it easier to maintain and scale your infrastructure as code  

### Terraform commands and console

- We can see the list of all commands in Terraform by typing `terraform`

- **terraform init**
    - Used to initialize a Terraform configuration. It is one of the first commands you should run when working on a new Terraform project.
    - Downloads and installs any necessary providers and modules that your Terraform configuration depends on. 
    - Terraform creates a lock file *.terraform.lock.hcl* to record the specific versions of the providers and modules used in your configuration. 


- **terraform plan**
    Used to create a plan(changeset) about the state of the infrastructure and what will be changed.

- **terraform apply**
    - Runs the plan and sends the plan to be executed by Terraform. It prompts for a *yes* or *no* to proceed further.
    - *To automatically approve the prompt, you can add `--auto-approve` to skip the same.*

- **terraform destroy**
    - Deletes the resources in your infrastructure.    

### Terraform lock files

`terraform.lock.hcl` contains locked versioning of the providers and modules that are used in the project.
Should be committed to the VSC(Version source control) ,i.e, GitHub

### Terraform State files

`terraform.tfstate` contains info about the current state of the infrastructure.
**Should not be committed to the VSC**, it contains sensitive data.

`terraform.tfstate.backup` is the previous state file

### Terraform directory

Contains binaries of the providers

### Issue with Terraform cloud login with Gitpod

When trying to run `terraform init`, a bash window opens up to generate a token but it does not work as expected. 

Workaround is to manually generate a token in Terraform cloud.\

```
https://app.terraform.io/app/settings/tokens?source=terraform-login

```

Creating and opening the file manually using the termimnal:

```sh
touch /home/gitpod/.terraform.d/credentials.tfrc.json
open /home/gitpod/.terraform.d/credentials.tfrc.json
```

Provide the following code (replacing your token in the file):

```json
{
  "credentials": {
    "app.terraform.io": {
      "token": "YOUR_TERRAFORM_CLOUD_TOKEN"
    }
  }
}

```

