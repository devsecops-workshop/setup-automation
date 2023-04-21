# DevSecOps Workshop - Automated Setup

Ansible automation to install and configure components of the workshop. For example to test the workshop or skip to certain chapters.

Currently supported OpenShift version 4.12

Currently supported component installation:

- Gitea (including repos)
- ODF
- Quay
- Quay Bridge
- DevSpaces
- OpenShift Pipelines
- OpenShift GitOps
- ACS
- ACM

## Run Options

The current version only supports running the playbooks in an Ansible Executing Environment a.k.a a container.

You can run the environment either on your local machine with access to a bastion host or on the bastion host directly(for example a RHPDS bastion host)

## Install ansible-navigator

The automation needs to be run with the `ansible-navigator` command.

### Installation Red Hat Enterprise Linux 8

You will need a valid Red Hat Subscription, then you can subscribe your RHEL host by

```
subscription-manager register

# get pool id via:
# subscription-manager list --available
subscription-manager attach [--auto] --pool=...

subscription-manager repos --disable=*

subscription-manager repos \
    --enable=rhel-8-for-x86_64-baseos-rpms \
    --enable=rhel-8-for-x86_64-appstream-rpms \
    --enable=rhel-8-for-x86_64-highavailability-rpms \
    --enable=ansible-automation-platform-2.1-for-rhel-8-x86_64-rpms


yum install -y ansible-navigator git podman

```

Note: If using a bastion on RHPDS or RHDP environments you need to unregister from the lab Satellite server and switch to the Red Hat cdn base url:

```
[root@bastion ~]$ subscription-manager unregister
[root@bastion ~]$ cp /etc/rhsm/rhsm.conf.kat-backup /etc/rhsm/rhsm.conf
```

This is the easiest way, if there is no rhsm.conf backup put this in your rhsm.conf:

```
[rhsm]
# Content base URL:
baseurl = https://cdn.redhat.com
```

Then proceed as above.

### Installation Centos Stream 8

Ansible navigator installation based on the upstream [documentation](https://ansible-navigator.readthedocs.io/en/latest/installation/#install-ansible-navigator).

```bash
dnf install -y python3-pip podman git
python3 -m pip install ansible-navigator --user
echo 'export PATH=$HOME/.local/bin:$PATH' >> ~/.profile
source ~/.profile

```

## Clone and prepare the project

You are now ready to clone this project to your system (local or bastion).

```
git clone https://github.com/devsecops-workshop/setup-automation.git
cd setup-automation
```

## Copy the inventory template and customize it

If running Ansible from a laptop or else:

```
cp inventory/custom_hosts.ini.template inventory/custom_hosts.ini
```

and replace <SSH JUMPHOST HOSTNAME> with the bastion hostname and <SSH_USER> and <SSH_PASSWORD>

If running Ansible on the bastion host, the inventory should look like:

```
<internal bastion IP> ansible_user=lab-user ansible_password=<SSH password>
```

## Check and adjust the path to kubeconfig on your bastion

The path is defined in `vars.yml` as _remote_kubeconfig_path_.

The default matches the position in a RHPDS bastion, so adjust if required.

## Run the automation

```
ansible-navigator run -m stdout ansible/setup.yml -i inventory/custom_hosts.ini
```

### Customize the automation

#### Installed components

By default the script will install all components.

You can choose to skip certain components by applying tags

The following tags are available

- prepare (must always be executed)
- acm
- devspaces
- gitea
- gitea_repo
- odf
- quay
- acs
- pipelines
- gitops

For example this command will skip the DevSpaces and ACM installation

```
ansible-navigator run -m stdout ansible/setup.yml -i inventory/custom_hosts.ini --skip-tags=devspaces, acm
```

#### Other configurations

You can further adjust the installation via environment variables

The following environment variables are available to be overwritten

- gitea_ssl: true

For example to install Gitea without SSL you can use to following command

```
ansible-navigator run -m stdout ansible/setup.yml -i inventory/custom_hosts.ini -e '{"gitea_ssl":false}'
```

# Optional : Build you own Execution Environment

In case you need to adjust the environment by adding additional tools you can build you own EE image

## Build Ansible execution with Podman

This will build a local version of the EE image

```bash
ansible-builder build \
    --container-runtime podman \
    --tag quay.io/sifa/devsecops-workshop:devel

podman push quay.io/sifa/devsecops-workshop:devel
```
