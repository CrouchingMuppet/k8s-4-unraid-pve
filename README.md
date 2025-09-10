# OpenShift (OKD) Ansible

This project was created for educational purposes, it is not intended for
production use of any kind.

I needed to re-learn Ansible after being away for some time 😮‍💨 but also start
familiarising myself with the OpenShift platform.

Note: I am using [OKD](https://okd.io) with Fedora Core images, not RedHat
OpenShift on RHEL. The concept should work with either, however.

This code currently allows the deployment of a HA or non-HA cluster on either
[Proxmox VE](https://proxmox.com/products/proxmox-virtual-environment/overview)
or [Unraid](https://unraid.net), the two platforms available to me in my home
lab.

- 🚧 Consider this project a work-in-progress
- 🚨 Error handling has not been fully tested
- 😭 I offer no guarantees that this code won't cause you some kind of grief
- ⚠️ Consider yourself warned

## How to use?

I make use of [mise](https://mise.jdx.dev/) to keep things contained. There are
other tools that work with Python, so feel free to use whatever, with whatever
on whatever that makes sense to you.

1. Install mise `brew install mise` _(optional)_
1. Download your pull secret: https://console.redhat.com/openshift/install/pull-secret
1. Place the text file in this folder (it is git-ignored)
1. If you are using mise, run `mise trust` in this folder _(optional)_
1. Install Ansible requirements `ansible-galaxy install -r requirements.yml`
1. Adjust the files below to suit your environment:
   - `host_vars/*`
   - `inventory/*`
   - `loadbalancer/defaults/main.yml`
   - `okd-cluster/defaults/main.yml`

There are four main playbooks:

```sh
# Create OKD infra within Proxmox
ansible-playbook create.yml -l pve
# Remove OKD infra from Proxmox
ansible-playbook destroy.yml -l pve
# Remove everything including OKD releases
ansible-playbook destroy.yml -l pve -e "persistence_cleanup=true"

# Create OKD infra within Unraid
ansible-playbook create.yml -l unraid
# Remove OKD infra from Unraid
ansible-playbook destroy.yml -l unraid
# Remove everything including OKD releases
ansible-playbook destroy.yml -l unraid -e "persistence_cleanup=true"
```
