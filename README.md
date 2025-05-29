# ansible-deploy-vmware-guest

Role to deploy VMware virtual machine from templates using Ansible

# Prerequisites

Install the Python modules :
    - Requests
    - PyVmomi

# Installation

You can add this role by doing a simple git clone

```
# File requirement.txt
---
- name: domain.role
  src: https://github.com/remylarrieu/Ansible_vSphere_template
  scm: git
  version: main

# Play
---
- name: Install ansible roles
  hosts: localhost
  tasks:
    - name: Install roles from requirements.yml
      ansible.builtin.command:
        cmd: ansible-galaxy install -r requirements.yml
      tags:
        - INSTALL_ROLES
```

# Configuration

You must edit the main.yml file in the vars folder in order to set the variables needed for the playbook file, such as:
 * **vsphere_host**, the address/hostname of the vcenter server
 * **vsphere_user**, a username with enough vSphere privileges to create VMs from templates (varibale in a vault file to secure this information)
 * **vsphere_password**, password of the  (variable in a vault file to secure this information)
 * **vsphere_datacenter**, the vSphere datacenter name
 * **vsphere_datastore**, the disk where to store the VM files
 * **vsphere_folder**, the folder where to drop VM ("/TO_DEPLOY" for ex.)
 * **guest_network**, **guest_netmask**, **guest_gateway**, **guest_dns_server**, **guest_domain_name**, the network informations for your VM
 * **guest_id** depending on the OS you want to deploy
 * **guest_memory** **guest_vcpu** for your VM sizing
 * **guest_template**, the template name to copy the VM from
 * **esxi_host**, the host where the VM will be deployed
 * **notes**, some description and information about the VM
 * **ansible_host_group**, the group in the inventory to add the host to ansible

Edit the **hosts_to_deploy** file that will be a temporary inventory file to store the hostname to deploy and the ip address (**guest_custom_ip**) of the new VM

Examples of theses files can be found inside this repository, under **vars/vars.sample.yml**.

# Execution

```
    ansible-playbook -i hosts_to_deploy deploy_vsphere_vm.yml --ask-vault-pass
```
