# Terraform Beginner Bootcamp 2023

## Table of Contents

- [Semantic Versioning](#semantic-versioning)
- [Install the Terraform CLI](#install-the-terraform-cli)
    - [Considerations with the Terraform CLI changes](#considerations-with-the-terraform-cli-changes)
    - [Refactoring into Bash Scripts](# 

## Semantic Versioning 🧙

This project is going to utilize semantic versioning for its tagging.
[semver.org](https://semver.org/)


The general format:

**MAJOR.MINOR.PATCH**, eg. `1.0.1`

- **MAJOR** version when you make incompatible API changes
- **MINOR** version when you add functionality in a backward compatible manner
- **PATCH** version when you make backward compatible bug fixes

## Install the Terraform CLI

### Considerations with the Terraform CLI changes
The Terraform CLI installation instructions have changed due to gpg keyring changes. So we needed to refer to the latest install CLI instructions via Terraform doumentation and change the scripting for install.

[Install Terraform CLI](https://developer.hashicorp.com/terraform/tutorials/aws-get-started/install-cli)

### Considerations for Linux Distrobution

This project is built against Ubuntu.
Please consider checking your Linux Distrobution and change accordingly to your distrobution needs.

[How to check OS version in Linux](https://www.cyberciti.biz/faq/how-to-check-os-version-in-linux-command-line/)

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

### Refactoring into BASH Scripts

While fixing the Terraform CLI gpg deprecation issues we noticed the bash script steps were a considerable amount more code. So we decided to create a bash script to install the Teraform CLI.

This bash sript is located here: [./bin/install_terraform_cli](./bin/install_terraform_cli)

- The will keep the Gitpod Task File ([.gitpod.yml](.gitpod.yml)) tidy.
- This allows us an easier way to debug and execute a manual Terraform CLI install.
- This will allow abetter portability for other projects that need to install Terraform CLI.

### Shebang

A Shebang (pronouced Sha-bang) tells the bash script what program that will interpret the script. eg. `#!/bin/bash`

ChatGPT recommended we use the format for bash: `#!/usr/bin/env bash`

- for portability for different OS distrobutions
- will search for user's PATH for the bash executable

https://en.wikipedia.org/wiki/Shebang_(Unix)
#### Execution Considerations

When executing the bash script we can use the `./` shorthand to exeute the bash script.

eg. `./bin/install_terraform_cli`

If we are using a script in .gitpod.yml we need to point the script to a program to interpret it.

eg. `source ./bin/install_terraform_cli`


#### Linux Permission Considerations

In order to make our bash scripts executable we need to change linux permissions for the fix to be executable at the user mode.

```sh
chmod u+x ./bin/install_terraform_cli
```

alternatively:

```sh
chmod 744 ./bin/install_terraform_cli
```

https://en.wikipedia.org/wiki/Chmod

#!/bin/bash
#!/usr/bin/env bash

### Github Lifecycle (Before, Init, Command)

We need to be careful when wusing the Init because it will not rerun if we restart an existing workspace.

But

https://www.gitpod.io/docs/configure/workspaces/workspace-lifecycle

### Working with Env Vars

We can list out all environment variables (Env Vars) using `env` command

We can filter specific env vars using grep eg. `env | grep AWS_`

#### Setting and Unsetting Env Vars

In the terminal we can set using `export HELLO='world`

In the terminal we can unset using `unset HELLO`

We can set an env var temporarily when just running a command

```sh
HELLO='world' ./bin/print_message
```

Within a bash script you can set an env without writing export eg.

```sh
#!/usr/bin/env bash

HELLO='world'

echo $HELLO
```

#### Printing Vars

We can print an env var using echo eg. `echo $HELLO`

#### Scoping of ENV Vars

When you open up new bash terminals in VScode, it will not be aware of env vars that you have set in another window.

If you want ENV Vars to persists across all future bash terminals that are open, you need to set env vars in your bash profile. eg. `.bash_profile`

#### Persisting Env Vars in Gitpod

We can persist env vars into gitpod by storing them in Gitpod Secrets Storage.

```
gp env HELLO='world
```

All future workspaces launched will set the env vars for all bash terminals opened in those workspaces.

You can also set env vars in the `.gitpod.yml` but this can only contain non-sensitive env vars.

### AWS CLI Installation

AWS CLI is installed for the project via the bash script [`./bin/install_aws_cli`](./bin/install_aws_cli)

[Getting Started Install (AWS CLI)](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html)
[AWS CLI Env Var](https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-envvars.html)
We can check if our AWS credentials is configured correctly by running the following AWS CLI command:
```sh
aws sts get-caller-identity
```

If it is successful you should see a json payload return that looks like this:
```json
{
"UserId": "AIDAYKLHY5TABWUREGDTCN",
"Account": "464968731254",
"Arn": "arn:aws:iam::3546984135149684DFGDFS:user/terraform-beginner-bootcamp"
}
```

We will need to generate AWS CLI credentials from IAM user in order to use the AWS CLI.


#### Terraform Basics

#### Terraform Registry

Terraform sources their providers and modules from the Terraform registry which is located at [registry.terraform.io](https://registry.terraform.io/)

- **Providers** is an interface to API's that will allow you to create resources in terraform.
- **Modules** are a way to refactor or make larges amounts of terraform code modular, portable and shareable.

#### Terraform Console

We can see a list of all the Terraform commands by simply typing 'terraform'

#### Terraform Init

`terraform init`

At the start of a new terraform project we will run `terraform init` to download the binaries for the terraform providers that we'll use in this project.

#### Terraform Plan

`terraform plan`

This will generate out a change set, about the state of our infrastructure and what will be changed.

We can output this change set ie. "plan" to be passed to an apply, but often you can just ignore outputting.

#### Terraform Apply

`terraform apply`

This will run a plan and pass the change set to the executed by terraform. Apply should prompt us to choose yes or no.

If we want to automatically approve an apply, we can provide the --auto-approve flag eg. `terraform apply --auto-approve.`

#### Terraform Destroy

`terraform destroy`
This will destroy resources 

You can also use the auto approve flag to skip the approve prompt eg.
`terraform apply --auto-approve`

#### Terraform Lock Files

`.terraform.lock.hcl` contains the locked versioning for the providers or modules that should be used with this project.

The Terraform Loc File should be committed to your Version Control System (VCS) eg. Github

#### Terraform State Files

`.terraorm.tfstate` contains information about the current state o your infrastructure.

This file **should not be commited** to your VCS.

This file can contain sensitive data.

If you lose this file, you will lose knowledge of the stat on your infrastructure.

`.terraform.tfstate.backup` is the previous state file state.

### Terraform Directory

`.terraform` directory contains binaries of terraform providers.

#### Issues with Terraform Cloud Login and Gitpod Workspace

When attempting to run `terraform login` it will launch bash in a weird view to generate a token. However it does not work as expected in Gitpod Vscode in the browser.

The workaround is to manually generate a token in Terraform Cloud

```
https://app.terraform.io/app/settings/tokens?source=terraform=login
```

Then create the file manually here:

```
touch /home/gitpod/.terraform.d/credentials.tfrc.json
open /home/gitpod/.terraform.d/credentials.tfrc.json
```

Provide the following code (replace your token in the file):

Then Open the file

We have automated the rpocess using this workaround with the following bash script [bin/generate_tfrc_credentials](bin/generate_tfrc_credentials)
