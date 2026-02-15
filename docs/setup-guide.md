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
