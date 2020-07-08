# Development Process

- [Development Process](#development-process)
  - [Overview](#overview)
  - [Getting started Script](#getting-started-script)
    - [Step by step installation process](#step-by-step-installation-process)
    - [One liner installation](#one-liner-installation)
  - [Build local environment](#build-local-environment)
    - [Docker Container for Ansible Testing and Development](#docker-container-for-ansible-testing-and-development)
      - [Development containers](#development-containers)
      - [Container commands](#container-commands)
    - [Python Virtual Environment](#python-virtual-environment)
      - [Install Python3 Virtual Environment](#install-python3-virtual-environment)
  - [Development & test tools](#development--test-tools)
    - [Pre-commit hook](#pre-commit-hook)
      - [Installation](#installation)
      - [Run pre-commit manually](#run-pre-commit-manually)
    - [Configure git hook](#configure-git-hook)

## Overview

Two methods can be used get Ansible up and running quickly with all the requirements to leverage ansible-avd.
A Python Virtual Environment or Docker container.

The best way to use the development files, is to copy them to the root directory where you have your repositories cloned.
For example, see the file/folder structure below.

```shell
├── git_projects
│   ├── ansible-avd
│   ├── ansible-cvp
│   ├── ansible-avd-cloudvision-demo
│   ├── <your-own-test-folder>
│   ├── Makefile
...
```

## Getting started Script

### Step by step installation process

```shell
mkdir git_projects
cd git_projects
git clone https://github.com/aristanetworks/ansible-avd.git
git clone https://github.com/aristanetworks/ansible-cvp.git
git clone https://github.com/arista-netdevops-community/ansible-avd-cloudvision-demo.git
cp ansible-avd/development/Makefile ./
make run
```

### One liner installation

One liner script to setup a development environment. it does following actions:

- Create local folder for development
- Instantiate a local git repository (no remote)
- Clone AVD and CVP collections
- Deploy Makefile

```shell
$ sh -c "$(curl -fsSL https://raw.githubusercontent.com/aristanetworks/ansible-avd/devel/development/install.sh)"
```

## Build local environment

### Docker Container for Ansible Testing and Development

The docker container approach for development can be used to ensure that everybody is using the same development environment while still being flexible enough to use the repo you are making changes in. You can inspect the Dockerfile to see what packages have been installed.
The container will mount the current working directory, so you can work with your local files.

The ansible version is passed in with the docker build command using ***ANSIBLE*** variable.  If the ***ANSIBLE*** variable is not used the Dockerfile will by default set the ansible version to 2.9.2

Before you can use a container, you must install [__Docker CE__](https://www.docker.com/products/docker-desktop) and [__docker-compose__](https://docs.docker.com/compose/) on your workstation.

#### Development containers

- [Ansible shell](https://hub.docker.com/repository/docker/avdteam/base): provide a built-in container with all AVD and CVP requirements already installed.
- [MKDOCS](https://github.com/titom73/docker-mkdocs) for documentation update: Run MKDOCS in a container and expose port `8000` to test and validate markdown rendering for AVD site.

#### Container commands

In this folder you have a [`Makefile`](./Makefile) providing a list of commands to start a development environment:

- `run`: Start a shell within a container and local folder mounted in `/projects`
- `dev-start`: Start a stack of containers based on docker-compose: 1 container for ansible playbooks and 1 container for mkdocs
- `dev-stop`: Stop compose stack and remove containers.
- `dev-run`: Connect to ansible container to run your test playbooks.
- `dev-reload`: Run stop and start.

If you want to test a specific ansible version, you can refer to this [dedicated page](https://github.com/arista-netdevops-community/docker-avd-base/blob/master/USAGE.md) to start your own docker image. You can also use following make command: `make ANSIBLE_VERSION=2.9.3 run`

Since docker image is now automatically published on [__docker-hub__](https://hub.docker.com/repository/docker/avdteam/base), a dedicated repository is available on [__Arista Netdevops Community__](https://github.com/arista-netdevops-community/docker-avd-base).

### Python Virtual Environment

#### Install Python3 Virtual Environment

```shell
# install virtualenv via pip3
$ sudo pip3 install virtualenv
...
```

```shell
# Configure Python virtual environment
$ virtualenv -p python3 .venv
$ source .venv/bin/activate

# Install Python requirements
$ pip install -r requirements.txt
```

## Development & test tools

### Pre-commit hook

[`pre-commit`](../.pre-commit-config.yaml) can run standard hooks on every commit to automatically point out issues in code such as missing semicolons, trailing whitespace, and debug statements. By pointing these issues out before code review, this allows a code reviewer to focus on the architecture of a change while not wasting time with trivial style nitpicks.

Repository implements following hooks:

- `trailing-whitespace`: Fix trailing whitespace. if found, commit is stopped and you must run commit process again.
- `end-of-file-fixer`: Like `trailing-whitespace`, this hook fix wrong end of file and stop your commit.
- `check-yaml`: Check all YAML files ares valid
- `check-added-large-files`: Check if there is no large file included in repository
- `check-merge-conflict`: Validate there is no `MERGE` syntax related to a invalid merge process.
- `pylint`: Run python linting with settings defined in [`pylintrc`](../pylintrc)
- `yamllint`: Validate all YAML files using configuration from [`yamllintrc`](../.github/yamllintrc)
- `ansible-lint`: Validate yaml files are valid against ansible rules.

#### Installation

`pre-commit` is part of [__developement requirememnts__](./requirements-dev.txt). To install, run `pip command`:

```shell
$ pip install -r requirements-dev.txt
...
```

#### Run pre-commit manually

To run `pre-commit` manually before your commit, use this command:

```shell
pre-commit run
[WARNING] Unstaged files detected.

[INFO] Stashing unstaged files to /Users/xxx/.cache/pre-commit/patch1590742434.

Trim Trailing Whitespace.............................(no files to check)Skipped
Fix End of Files.....................................(no files to check)Skipped
Check Yaml...........................................(no files to check)Skipped
Check for added large files..........................(no files to check)Skipped
Check for merge conflicts............................(no files to check)Skipped
Check for Linting error on Python files..............(no files to check)Skipped
Check for Linting error on YAML files................(no files to check)Skipped
Check for ansible-lint errors............................................Passed

[INFO] Restored changes from /Users/xxx/.cache/pre-commit/patch1590742434.
```

Command will automatically detect changed files using git status and run tests according their type.

### Configure git hook

To automatically run tests when running a commit, configure your repository whit command:

```shell
$ pre-commit install
pre-commit installed at .git/hooks/pre-commit
```

To remove installation, use `uninstall` option.
