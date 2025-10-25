# Kubernetes for Unraid and Proxmox VE

This project was created for educational purposes, it is not intended for
production use of any kind.

This code currently allows the deployment of a HA or non-HA Kubernetes,
Openstack or OpenShift cluster on [Proxmox VE](https://proxmox.com/products/proxmox-virtual-environment/overview)
and [Unraid](https://unraid.net). The playbooks mostly rely on the available
packages for the OS. This is important for Unraid which does not have any form
of standard package management.

🚧 Consider everything here a work-in-progress. There are no guarantees this code won't cause you some kind of grief 😭

## How to use?

### Prerequisites

This project assumes the use of [mise](https://mise.jdx.dev/) and [UV](https://github.com/astral-sh/uv) for Python management. Feel free to
use whatever equivalent tooling you prefer.

1. Install mise `brew install mise`
1. Run `mise trust` in this folder
1. Install Ansible `uv install ansible`
1. Install Ansible collections `uv run ansible-galaxy install -r requirements.yml`

⚠️ When customising configuration, please give special attention to
`kube_config_dest` as any existing file will be overwritten ⚠️

⚠️ `k8s` flavour does not currently have HA etcd cluster ⚠️
⚠️ `openstack` flavour is incomplete ⚠️

There are two main playbooks, use with:

- `--limit` targets desired host group (`pve` or `unraid`)
- `--tags` determines your flavour of Kubernetes, (currently `okd`, `k3s`, `k8s` and `openstack` are supported)

There is configuration you should check:

- `host_vars/*`
- `inventory/*`
- `loadbalancer/defaults/main.yml`

### OpenShift (OKD)

1. Download your pull secret: https://console.redhat.com/openshift/install/pull-secret
1. Place the text file in this folder (it is git-ignored)
1. Check defaults in:
   - `okd-cluster/defaults/main.yml`
1. Run a playbook

e.g.

```sh
# Deploy/destroy OKD cluster on Proxmox VE
uv run ansible-playbook create.yml --limit pve --tags okd
uv run ansible-playbook destroy.yml --limit pve --tags okd

# Deploy/destroy OKD on Unraid with optional cleanup
uv run ansible-playbook create.yml --limit unraid --tags okd
uv run ansible-playbook destroy.yml --limit unraid --tags okd --extra-vars "persistence_cleanup=true"
```

### Rancher k3s

1. Check defaults in:
   - `cluster-vms/defaults/main.yml`
1. If using db-backed etcd, change `k3s/defaults/main.yml`
1. Run a playbook using tags `k3s`

e.g.

```sh
uv run ansible-playbook create.yml --limit pve --tags k3s
uv run ansible-playbook destroy.yml --limit pve --tags k3s
```

### Standard k8s

1. Check defaults in:
   - `cluster-vms/defaults/main.yml`
   - `k8s-cluster/defaults/main.yml`
1. Run a playbook using tags `k8s`

e.g.

```sh
uv run ansible-playbook create.yml --limit unraid --tags k8s
uv run ansible-playbook destroy.yml --limit unraid --tags k8s --extra-vars "persistence_cleanup=true"
```

### Openstack

1. Check defaults in:
   - `cluster-vms/defaults/main.yml`
1. Run a playbook using tags `openstack`

e.g.

```sh
uv run ansible-playbook create.yml --limit unraid --tags openstack
uv run ansible-playbook destroy.yml --limit unraid --tags openstack
```
