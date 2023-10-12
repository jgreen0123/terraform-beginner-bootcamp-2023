# Terraform Beginner Bootcamp 2023

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