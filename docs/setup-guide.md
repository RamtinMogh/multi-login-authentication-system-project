# Setup Guide - Multi-Login Authentication System (AD DS + SSSD)

This guide follows the exact implementation steps from the original project proposal (pages 10–15, Technical Documentation).

## Lab Environment
- VMware Workstation
- VMs:
  - Windows Server 2019 (domain controller - bigguy.local)
  - Linux Mint 1 (client - 192.168.100.30)
  - Linux Mint 2 (client - 192.168.100.40)
  - Windows 10 (client - 192.168.100.20)
- All VMs on the same virtual network

## Step 1: Windows Server 2019 Setup (Domain Controller)
Set static IP:
192.168.100.10/24

Launch Server Manager → Add Roles and Features:
- Active Directory Domain Services (AD DS)
- DNS Server

Promote to domain controller:
- Add a new forest
- Root domain name: bigguy.local
- Set Directory Services Restore Mode (DSRM) password
- Accept default NetBIOS name

After reboot, open Active Directory Users and Computers (ADUC):
- Create users: shareduser, restricteduser1

## Step 2: Linux Mint Client Setup (Mint 1 and Mint 2)
Switch to NAT for internet access (temporary):
- Advanced Network Configuration → Edit adapter → IPv4 Settings → Method: Automatic (DHCP)

Install required packages:

```bash
sudo apt update
sudo apt install realmd sssd sssd-tools libnss-sss libpam-sss krb5-user packagekit adcli oddjob oddjob-mkhomedir -y
```

Switch to Host-only networking (after packages installed):
- Advanced Network Configuration → Edit adapter → IPv4 Settings → Method: Manual
- Assign static IP: Linux Mint 1 - 192.168.100.30, Linux Mint 2 - 192.168.100.40
- Subnet mask: 255.255.255.0
- DNS: 192.168.100.10 (Windows Server)


Update /etc/resolv.conf:

```bash
nameserver 192.168.100.10
options edns0 trust-ad
search bigguy.local
```

Join the domain:

```bash
sudo realm join -U Administrator bigguy.local
```
