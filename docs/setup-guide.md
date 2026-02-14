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

Restart SSSD to apply changes:

```bash
sudo systemctl restart sssd
```

Enable automatic home directory creation on first login (recommended):
```bash
sudo pam-auth-update --enable mkhomedir
```



## Step 3: Configure SSSD for Authentication and Identity
Edit /etc/sssd/sssd.conf (use sudo nano /etc/sssd/sssd.conf):

```ini
[sssd]
domains = bigguy.corp
config_file_version = 2
services = nss, pam

[domain/bigguy.corp]
id_provider = ad
auth_provider = ad
access_provider = ad
cache_credentials = True
krb5_realm = BIGGUY.CORP
realmd_tags = manages-system joined-with-adcli
fallback_homedir = /home/%u@%d
use_fully_qualified_names = True
