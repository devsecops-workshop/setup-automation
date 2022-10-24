# setup-automation

Ansible automation to install parts of the workshop components

## Usage

- Make sure you have Ansible installed
- Copy inventory.ini.template to inventory.ini
- Enter the SSH Jumphost hostname (this should be a system with the oc cli and direct access to your Openshift cluster), SSH username and password
- Run the playbook e.g. `ansible-playbook setup-playbook.yml`
