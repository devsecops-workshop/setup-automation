# setup-automation

Ansible automation to install parts of the workshop components

## In case of Red Hat Enterprise Linux 8

Subscribe your RHEL host:
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

## In case of Centos 8

Ansible navigator installation based on the upstream [documentation](https://ansible-navigator.readthedocs.io/en/latest/installation/#install-ansible-navigator).

```bash
dnf install -y python3-pip podman git
python3 -m pip install ansible-navigator --user
echo 'export PATH=$HOME/.local/bin:$PATH' >> ~/.profile
source ~/.profile

```

## Initialize tools

You are now ready to clone this project to your CentOS or RHEL system.

```
git clone https://github.com/devsecops-workshop/setup-automation.git
```

## Setup DevSecops-Workshop components

```
cd setup-automation
ansible-navigator run -m stdout ansible/setup.yml
```

# Build / Development

## Build ansible execution enviorment

```bash
ansible-builder build \
    --container-runtime podman \
    --tag quay.io/sifa/devsecops-workshop:devel

podman push quay.io/sifa/devsecops-workshop:devel
```