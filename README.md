# Multi-Login Authentication System (AD DS + SSSD)

**Centralized authentication for hybrid Windows/Linux environments** — Enables seamless logins, user/group management, and permissions across Windows Server 2019 and Linux Mint using Active Directory Domain Services (AD DS) and System Security Services Daemon (SSSD). Built as a Seneca College project to provide enterprise-level identity management.

[![Windows Server](https://img.shields.io/badge/Windows_Server-2019-blue?logo=windows)](https://learn.microsoft.com/en-us/windows-server/)
[![Linux Mint](https://img.shields.io/badge/Linux-Mint-green?logo=linux)](https://linuxmint.com/)
[![AD DS](https://img.shields.io/badge/AD_DS-Active_Directory-red)](https://learn.microsoft.com/en-us/windows-server/identity/ad-ds/)
[![SSSD](https://img.shields.io/badge/SSSD-System_Security-yellow)](https://sssd.io/)
[![VMware](https://img.shields.io/badge/VMware-Workstation-blue)](https://www.vmware.com/products/workstation-pro.html)

## Overview
This project implements a multi-login authentication system for BigGuy Corp's mixed OS environment:
- Windows Server 2019 as domain controller with AD DS for centralized user/group management
- Linux Mint clients joined to the domain using SSSD for Kerberos authentication and LDAP identity lookup
- Windows 10 clients also joined to the domain
- Supports single sign-on (SSO), NTFS/share permissions synced to Linux, and selective access
- Zero-cost, fully functional in a virtual lab

Full project proposal: [BigGuy Corp Multi-Login Authentication Proposal](docs/BigGuy-Corp-Authentication-Proposal.pdf)

## Goals
- Enable centralized user authentication across Windows and Linux
- Allow seamless logins from Linux Mint clients using AD credentials
- Manage users/groups/permissions from one place (AD DS)
- Demonstrate secure authentication (Kerberos) and identity lookup (LDAP)
- Map permissions and access controls (NTFS/share to Linux)
- Test password changes and selective access

## Technologies Used
- Windows Server 2019 — AD DS, DNS, DHCP, GPO
- Linux Mint — SSSD, realmd, Kerberos, PAM
- VMware Workstation — virtual lab (domain controller, clients)
- Kerberos — Secure ticket-based authentication
- LDAP — Identity lookup

## What I Built / Implemented
- Configured Windows Server as domain controller (bigguy.local) with AD DS, DNS, DHCP
- Created users/groups in AD DS (shareduser, restricteduser1)
- Joined Linux Mint clients to the domain using realmd and SSSD
- Configured SSSD.conf for identity, authentication, access, and selective filtering
- Edited lightdm.conf for manual login and no guest access
- Joined Windows 10 client to the domain and disabled cached logons
- Verified SSO logins, password sync, Kerberos tickets, and selective access (restricteduser1 denied on Mint 2)
- Documented full setup, configs, troubleshooting (DNS, time sync, firewall, realm join issues)

## Setup Guide
See detailed step-by-step in [docs/setup-guide.md](docs/setup-guide.md)

Quick overview:
1. Configure Windows Server as domain controller (AD DS + DNS)
2. Install SSSD/realmd on Linux Mint
3. Join Linux to domain: sudo realm join bigguy.local
4. Configure SSSD.conf and lightdm.conf
5. Join Windows 10 to domain
6. Test logins, permissions, and password changes

## Results
- AD users can log in to Linux Mint and Windows 10 with domain credentials
- Groups and selective access work (restricteduser1 denied on Mint 2)
- Kerberos tickets issued (klist)
- Password changes sync across clients
- DNS resolving correctly on Linux (nslookup bigguy.local)

## Challenges & Resolutions
- DNS/time sync issues → Configured NTP and DNS to point to domain controller
- Realm join failures → Fixed firewall, resolv.conf, and package dependencies
- Selective access on Mint 2 → Used simple_allow_users in SSSD.conf
- Cached logons on Windows 10 → Disabled via registry (CachedLogonsCount = 0)

## Repository Structure
- `docs/` → Problem statement, setup guide, configs, proposal PDF
- `README.md` → Project overview and documentation

This repo showcases my hands-on experience with Windows/Linux integration, AD DS administration, SSSD configuration, Kerberos authentication, domain join troubleshooting, and virtualization lab setup.
