# setup-automation

Ansible automation to install parts of the workshop components

## pre

- sudo pip3 install openshift pyyaml kubernetes
- sudo ansible-galaxy collection install community.kubernetes


## Usage

- Make sure you have Ansible installed
- Run the playbook e.g. `ansible-playbook setup.yml`



## Sifas Notes
oc get nodes | grep -v master | awk '{ print $1 }' | grep -v NAME