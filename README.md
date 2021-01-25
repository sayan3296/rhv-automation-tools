# RHV Automation Tools

[![License: GPLv2](https://img.shields.io/badge/license-GPLv2-brightgreen.svg)](https://www.gnu.org/licenses/old-licenses/gpl-2.0.en.html)
[![License: GPLv3](https://img.shields.io/badge/license-GPLv3-brightgreen.svg)](https://www.gnu.org/licenses/gpl-3.0)

Tools to automate RHV (Red Hat Virtualization) deployment and Day 2
operations. Hardware initialization and kickstarting from Satellite
currently not implemented but the playbook architecture allows that.

## Contents

* [ansible.cfg](ansible.cfg)
  * Example Ansible configuration
* [hw_cisco_config.yml](hw_cisco_config.yml)
  * Playbook to initialize Cisco hardware for RHV (unimplemented)
* [hw_cisco_verify.yml](hw_cisco_verify.yml)
  * Playbook to verify Cisco hardware status with RHV (unimplemented)
* [inventory_example.yml](inventory_example.yml)
  * Inventory for the playbooks
* [rhv_backup.yml](rhv_backup.yml)
  * Playbook to back and restore RHV Manager (test before use!)
* [rhv_config.yml](rhv_config.yml)
  * Main RHV playbook
* [rhv_deploy.yml](rhv_deploy.yml)
  * Playbook to initialize hardware and deploy RHV cluster
* [rhv_update.yml](rhv_update.yml)
  * Playbook to update RHV (not up-to-date for RHV 4.4)
* [rhv_user.yml](rhv_user.yml)
  * Playbook to add/delete RHV internal user (for example, for OCP)
* [rhv_verify.yml](rhv_verify.yml)
  * Playbook to verify RHV cluster status
* [vars/](vars/)
  * Environment and RHV specific configurations

Partially implemented [roles](roles) are included to backup and restore
RHV Manager (not up-to-date for RHV 4.4).

## RHV Host Installation

For hardware and other requirements for RHV please refer to Red Hat
documentation:

https://access.redhat.com/documentation/en-us/red_hat_virtualization/

Prepare for installation by downloading the latest RHV Hypervisor images
for RHV Hosts:

https://access.redhat.com/products/red-hat-virtualization

Initiate installation from the ISO image.

After installation ensure the required storage and network hardware is
available on all RHV Hosts.

## Automated RHV Cluster Setup with Ansible Playbooks

On a Linux host with Ansible installed edit the the
_inventory_example.yml_ file to match the local environment.

Enter the username/password details to allow registering the systems
automatically during the deployment. Alternatively, register the systems
manually and uncomment the roles from the playbook to avoid running
them.

Use the command `multipath -ll` to get the LUN IDs for the inventory
file in case using Fibre Channel SAN storage. Only FC and NFS storage
types have been tested with these playbooks.

Investigate files under [vars/](vars/) and customize as needed.

To prepare RHV Hosts use the following command (the tag is mandatory):

```
ansible-playbook rhv_config.yml -i ./inventory_example.yml -u root -t hosts
```

Ensure a clean run (it is safe to run the playbook several times), fix
any possible issues before continuing. Without a clean run it is highly
likely that the next step will fail.

To deploy RHV Manager and configure RHV cluster, run:

```
ansible-playbook rhv_config.yml -i ./inventory_example.yml -u root -t manager
```

Alternatively, the deploy RHV cluster with one command:

```
ansible-playbook rhv_deploy.yml -i ./inventory_example.yml -u root -t rhv
```

### RHV Finalization

Login to RHV Manager and verify the cluster looks healthy.

Especially pay attention to Storage and Network setup.

## TODO

For known TODO items:

```
grep -r TODO .
```

## License

GPLv2+
