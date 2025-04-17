# 🚀 SAP HANA Storage File System Provisioning with Ansible

This Ansible playbook automates the creation and configuration of file systems required for SAP HANA, including partitioning, LVM setup, formatting, mounting, and persisting mount points.

---

## 📦 Requirements

- Ansible control node (RHEL, CentOS, or Fedora)
- Passwordless SSH access to SAP HANA target nodes
- Attached virtual disks on target VMs (e.g. `/dev/sdb`, `/dev/sdc`, etc.)
- Community collection `community.general`

---

## 🛠️ Setup Guide

### 🔐 1. Set Up SSH Authentication

On the **Ansible control node**:

ssh-keygen -t rsa
Copy the public key to all managed nodes:

ssh-copy-id root@hana1
ssh-copy-id root@hana2
📁 2. Create Your Inventory File
Example: inventory.ini
cat /etc/ansible/hosts

[hana]
hana01 ansible_user=root ansible_ssh_private_key_file=~/.ssh/id_rsa
hana02 ansible_user=root ansible_ssh_private_key_file=~/.ssh/id_rsa
hana1  ansible_user=root ansible_ssh_private_key_file=~/.ssh/id_rsa
hana2  ansible_user=root ansible_ssh_private_key_file=~/.ssh/id_rsa
📚 3. Install Required Ansible Collections

ansible-galaxy collection install community.general
💽 4. Attach Virtual Disks to the VMs
Ensure your target VMs have additional disks (e.g. via vSphere, KVM, or cloud provider). These should appear as:

/dev/sdb
/dev/sdc
/dev/sdd, etc.
Check with:
lsblk

🚧 What the Playbook Does
Partitions each disk using GPT
Creates LVM volume groups and logical volumes
Formats each LV with xfs
Mounts them to appropriate SAP HANA directories:

/hana/data
/hana/log
/hana/shared
/usr/sap

Updates /etc/fstab for persistent mounts

▶️ How to Run
ansible-playbook -i inventory.ini sap_hana_storage.yml
