# Terraform Beginner Bootcamp 2023

## Semantic Versioning :mage:

This project is going to utilize semantic versioning for its tagging.
[semver.org](https://semver.org/)

The general format:

**MAJOR.MINOR.PATCH**, eg. `1.0.1`

- **MAJOR** version when you make incompatible API changes
- **MINOR** version when you add functionality in a backward compatible manner
- **PATCH** version when you make backward compatible bug fixes

## Install the Terraform CLI

### Considerations with the Terraform CLI changes
Thee Terraform CI installation instrustions have changed due to gpg keyring changes. So we needed to refer to the latest CLI insturctions via Terraform Documentation and chagne the scripting for install.

[Installing Terraform](https://developer.hashicorp.com/terraform/downloads)

## Refactoring into Bash Scripts

While fixing the Terraform CLI gpg deprecation issues we notice that bash scripts were a considerable amount more code. So we decided to create a bash script to install the terraform CLI.

Bash script located here: [./bin/install_terraform_cli](./bin/install_terraform_cli)

- This will keep the Gitpod Task file ([.gitpod.yml](.gitpod.yml)) tidy.
- This will allow us to easily debug and manually execute the Terraform CLI install.
- This will allow better portability for other projects that need to install the Terraform CLI.

## Considerations for Linux Distribution

This project is built against Ubuntu.
Please consider checking your Linux distribution and change accordingly to distribution needs.

[How to Find Linux Distro](https://www.howtogeek.com/206240/how-to-tell-what-distro-and-version-of-linux-you-are-running/)

Example of checking OS Version.
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

#### Shebang Considerations

A Shebang (pronounced Sha-bang) tells the bash script what program will interpret the script. eg. `#! /bin/bash`

ChatGPT recommended this format for bash: `#!/usr/bin/env bash`

- for portablitiy to different OS distributions
- will search the users PATH for the bash executable

## Execution Considerations

When executing the bash script we can use the `./` shorthand notation.

eg. `./bin/install_terraform_cli`

If we are using a script in .gitpod.yml we need to point the script to a program to interpret it.

#### Linux Permissions

In order to make out bash scripts executable  we need to change linux permission be to executable at the user mode.

```sh
chmod u+x ./bin/install_terraform_cli
```

or:
```sh
chmod 744 ./bin/install_terraform_cli
```

https://en.wikipedia.org/wiki/Chmod

### Gitpod Lifecycle (Before, Init, Command)

We need to be careful when using the Init because it will not rerun if we restart the machine.

https://www.gitpod.io/docs/configure/workspaces/workspace-lifecycle

### Working with Env Vars

We can list out all the Environment Variables (Env Vars) using the `env` command.

 We can filter specific env vars using grep eg. `env | grep AWS_`

 #### Setting and Unsetting Env Vars

 In the terminal we can set using `export Hello=world`

 In the terminal we can unset using `unset Hello`

 We can set an enc var temporarily when just running a command

 ```sh
 HELLO='world' ./bin/print_message
 ```

 Within a bash script we can set env var without writing export eg.

 ```
 #!/usr/bin/env bash

 HELLO='world'

 echo $HELLO

 ```

 ### Printing Vars

 We can print an env var using echo eg. `echo $HELLO`

 #### Scoping of Env Vars

 When you open up new bash terminals in VSCode it will not be aware of env vars that you have set in another window.

 If you want to have env vars to persist across all future bash terminals that are open you need to set env vars in your bash profile. eg. `.bash_profile`

 #### Persisting Env Vars in Gitpod

 We can persist env vars in Gitpod by storing them in Gitpod Secrets Storage

 ```
 gp env HELLO='world'
 ```

 All future workspaces launched will set the env vars for all bash terminals opened in those workspaces.

 You can also set env vars in the `.gitpod.yml` but this can only contain non-sensitive env vars.

 ### AWS CLI Installation

 AWS CLI is installed for the project via the bash script [`./bin/install_aws_cli`](./bin/install_aws_cli)

 [Getting Started Install (AWS CLI)](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html)

 [AWS CLI Env Vars](https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-envvars.html)

We can check if our AWS credentials are configured correctly by running the following command:

 ```sh
 aws sts get-caller-identity
 ```

 If it is successful you should see a json payload return that looks like this:

 ```json
 {
 "UserId": "AIDA4SX7D3BNQ777VSVZP",
    "Account": "123456789012",
    "Arn": "arn:aws:iam::123456789012:user/terraform-beginner-bootcamp"
 }
 ```
We'll need to generate AWS CLI credentials from IAM User in order to use the AWS CLI.


 