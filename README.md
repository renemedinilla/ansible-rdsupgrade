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
Each role can run independently, so each one has their own configurations and defaults, wich can be found in `roles/<roleName>/defaults/main.yml`. ALL of the roles defaults can be overwritten by updating the `group_vars/all/vars.yml` file.

Set following environment variables:
```
AWS_ACCESS_KEY_ID=<your-aws-access-key-id>
AWS_SECRET_ACCESS_KEY=<your-aws-secret-access-key>
AWS_REGION=<your-prefered-aws-region>
```

Run the initial deployment (creates a MySQL RDS instance with 5.5 engine):
```
ansible-playbook deploy.yml
```
Once the instance is created you can create the new instance by running:
```
ansible-playbook upgradeRds.yml
```

Which will create an RDS read replica with the master version (5.5) and then upgrade it to the next major version (5.6)

Once everything is completed a new file will be created in `facts/rds_out.json` with the properties of the new updated replica instance.

## Roles
This project consists in various roles, each one with it's own responsibility.
### deployRds
Creates the MySQL RDS master instance, using the `sourceInstanceName` default or variable.
### createUpgradedReplica
Initiates the RDS MySQL replica creation using the `replicaInstanceName` default or variable. After creation it will upgrade the engine version and save the file to the `facts` directory.