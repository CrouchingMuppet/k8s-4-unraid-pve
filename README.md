# Kubernetes for Unraid and Proxmox VE

This project was created for educational purposes, it is not intended for
production use of any kind.

This code currently allows the deployment of a HA or non-HA Kubernetes or
OpenShift cluster on [Proxmox VE](https://proxmox.com/products/proxmox-virtual-environment/overview)
and [Unraid](https://unraid.net). The playbooks mostly rely on the available
packages for the OS. This is important for Unraid which does not have any form
of standard package management.

- 🚧 Consider this project a work-in-progress
- 🚨 Error handling has not been fully tested
- 😭 I offer no guarantees that this code won't cause you some kind of grief

## How to use?

### Prerequisites

This project assumes the use of [mise](https://mise.jdx.dev/) for Python
version control. Feel free to replace the mise commands with [UV](https://github.com/astral-sh/uv)
or whatever equivalent tooling you prefer.

1. Install mise `brew install mise`
1. Run `mise trust` in this folder
1. Install Ansible `pip install ansible`
1. Install Ansible collections `ansible-galaxy install -r requirements.yml`

⚠️ When customising configuration, please give special attention to
`kube_config_dest` as any existing file will be overwritten ⚠️

There are two main playbooks, use with:

- `--limit` targets desired host group (`pve` or `unraid`)
- `--tags` determines your flavour of Kubernetes, (currently `okd`, `k3s` are supported)

### OpenShift (OKD)

1. Download your pull secret: https://console.redhat.com/openshift/install/pull-secret
1. Place the text file in this folder (it is git-ignored)
1. Adjust the files below to suit your environment:
   - `host_vars/*`
   - `inventory/*`
   - `loadbalancer/defaults/main.yml`
   - `okd-cluster/defaults/main.yml`

e.g.

```sh
# Deploy/destroy OKD cluster on Proxmox VE
ansible-playbook create.yml --limit pve --tags okd
ansible-playbook destroy.yml --limit pve --tags okd

# Deploy/destroy OKD on Unraid with optional retention of downloads
ansible-playbook create.yml --limit unraid --tags okd
ansible-playbook destroy.yml --limit unraid --tags okd --extra-vars "persistence_cleanup=false"
```

### Rancher k3s

1. Adjust the files below to suit your environment:
   - `host_vars/*`
   - `inventory/*`
   - `loadbalancer/defaults/main.yml`
   - `k3s-cluster/defaults/main.yml`

e.g.

```sh
# Deploy/destroy k3s cluster on Proxmox VE
ansible-playbook create.yml --limit pve --tags k3s
ansible-playbook destroy.yml --limit pve --tags k3s

# Deploy/destroy k3s on Unraid with optional retention of downloads
ansible-playbook create.yml --limit unraid --tags k3s
ansible-playbook destroy.yml --limit unraid --tags k3s --extra-vars "persistence_cleanup=false"
```
