# Setup Guide - Multi-Login Authentication System (AD DS + SSSD)

This guide follows the exact implementation steps from the original project proposal (pages 7–12).

## Lab Environment
- Hypervisor: VMware Workstation
- VMs:
  - Windows Server 2019 (domain controller - bigguy.corp)
  - Linux Mint (client - domain-joined)
- All VMs on the same virtual network

## Step 1: Configure Windows Server 2019 as Domain Controller
- Install AD DS role + DNS + DHCP
- Promote to domain controller (bigguy.corp)
- Create users and groups in Active Directory Users and Computers

## Step 2: Install and Configure SSSD on Linux Mint
Install the required packages for domain join and SSSD:

```bash
sudo apt update
sudo apt install realmd sssd sssd-tools adcli krb5-user samba-common-bin oddjob oddjob-mkhomedir packagekit -y
```

Join the Linux Mint client to the domain (replace Administrator with your domain admin user if different):

```bash
sudo realm join bigguy.corp --user=Administrator
```
Enter the domain admin password when prompted

Verify the domain join was successful:

```bash
realm list
id Administrator@bigguy.corp
```
