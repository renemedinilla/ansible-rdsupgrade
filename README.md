# Upgrade secondary replica using Ansible
This exerecise consists in the following:
- launch 5.5 RDS mysql master 5.5
- launch 5.5 RDS read replica from mysql master 5.5
- upgrade the read replica to 5.6 ( apply immediately ) 
- Collect the facts about newly started node and save them in ./facts directory
## Requirements
### Ansible
This exercise was created using **Ansible 2.5** on Ubuntu 16.04. In order to get ansible working on your local machine follow this [steps](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html#latest-releases-via-apt-ubuntu).

### Ansible modules:
- boto
- python >= 2.6
- aws cli

## Setting up and Running the project
Each role can run independently, so each one has their own configurations, wich can be found in `roles/<roleName>/defaults/main.yml`. Configure each one accordingly with your specific 

Set environment variables:
AWS_ACCESS_KEY_ID
//AWS_SECRET_KEY
AWS_SECRET_ACCESS_KEY
AWS_REGION >= 1.15
parameters like your aws keys.
```
ansible-playbook upgradeRds.yml
```

## Roles
This project consists in various roles, each one with it's own responsibility.
### deployRds
Initiates the RDS MySQL instance creation with a read reaplica in another AZ.
### upgradeReplica
Initiates the RdS replica upgrade process and finishes when the replicacion has succeeded.