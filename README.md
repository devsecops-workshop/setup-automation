# setup-automation

Ansible automation to install parts of the workshop components

## pre

- sudo pip3 install openshift pyyaml kubernetes
- sudo ansible-galaxy collection install community.kubernetes

## Usage

- Make sure you have Ansible installed
- If you want to execute on other hosts than localhost such as a Bastion/Jumphost, copy inventory/custom_hosts.ini.template to inventory/custom_hosts.ini and enter hostname and SSH credentials if required
- Run the playbook e.g. `ansible-playbook setup.yml` (localhost) or `ansible-playbook setup.yml -i inventory/custom_hosts.ini` (for your custom hosts)

## Sifas Notes

oc get nodes | grep -v master | awk '{ print $1 }' | grep -v NAME
