![Repo Size](https://img.shields.io/github/repo-size/Sulstice/global-chem)
[![python](https://img.shields.io/badge/python-3.12-blue.svg)](https://www.python.org)

# ansible-cisco-iosxe
This repository contains ansible playbooks that can be used for the following tasks on Cisco IOS-XE switches:

  * backup config and save running-config to startup-config
  * generate config
  * generate html report with general facts (model, software version, SN)
  * generate html report about ntp servers that are configured
  * generate html report about ports with port security enabled
  * configure switch with given template
  * verify config with master config

## Getting Started

### Prerequisites

  * Ansible only works in Linux environment so use the Linux distro of your choice, WSL inside windows can also be used
  * Have python installed
  * Have ansible installed
  * Make sure your user has permision to make changes in the ansible folder, needed for running playbooks and logging

Add the default values below to your absible.cfg file. (optional) This comes in handy if you don't use  ssh keys and want to enable logging.

```
[defaults]
host_key_checking = False
log_path = logging.log
```

Create log file (optional)

```
touch logging.log
```

Create virtual environment inside /etc/ansible/ and activate it

```
cd /etc/ansible/
sudo python3 -m venv venv 
source venv/bin/activate
```

Isntall the following python modules, they are needed to parse output from the switches

```
pip install pyats
pip install genie
pip install ansible
```

Clone ansible-pyats role into role directory

```
git clone https://github.com/CiscoDevNet/ansible-pyats "${ANSIBLE_ROLES_PATH:-roles}/ansible-pyats"
```

Remove the secrets_fault.yml file and create your own

```
ansible-vault create files/secret_info.yml
```

Edit file

```
ansible-vault edit files/secret_info.yml
```

Add info below as example

```
ansible_user: cisco
ansible_password: cisco
radius_key: radiuskey
```

Edit inventory.ini with your own test devices

```
[routers]
R1 ansible_host=192.168.178.54
R2 ansible_host=192.168.178.56
R3 ansible_host=192.168.178.55

[switches]
R4 ansible_host=192.168.178.57
R5 ansible_host=192.168.178.58
```

## Usage

Execute the playbooks from the base of folder 'ansible-cisco-iosxe'. Example:

```
ansible-playbook -i inventory.ini playbooks/iosxe_report_facts.yml --ask-vault-pass
```

Every playbooks has a description with it's use case and how to run it.
