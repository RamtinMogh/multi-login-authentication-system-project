# Multi-Login Authentication System (AD DS + SSSD)

**Problem**: In real enterprise environments, companies use Windows Active Directory (AD) for centralized user management, logins, and permissions. But Linux systems (like servers or workstations) don't natively integrate with AD — they can't log in with domain credentials, access shared resources, or respect AD permissions without complex manual workarounds or expensive third-party tools.

**Solution**: This project builds a complete hybrid authentication system that lets Linux Mint clients seamlessly log in with Active Directory credentials from a Windows Server 2019 domain controller, using open-source SSSD for domain join, Kerberos authentication, and LDAP identity lookup. It supports single sign-on (SSO), selective access (e.g., certain users denied on one client), password sync, and permission mapping — all in a virtual lab.

Built as a Seneca College project to solve the exact problem of "Linux can't log in to Windows AD domain" without proprietary software.

[![Windows Server 2019](https://img.shields.io/badge/Windows_Server-2019-blue?logo=windows)](https://learn.microsoft.com/en-us/windows-server/)
[![Linux Mint](https://img.shields.io/badge/Linux-Mint-green?logo=linux)](https://linuxmint.com/)
[![Active Directory](https://img.shields.io/badge/AD_DS-Active_Directory-red)](https://learn.microsoft.com/en-us/windows-server/identity/ad-ds/)
[![SSSD](https://img.shields.io/badge/SSSD-System_Security-yellow)](https://sssd.io/)
[![VMware](https://img.shields.io/badge/VMware-Workstation-blue)](https://www.vmware.com/products/workstation-pro.html)
[![MIT License](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)

## What the Project Solves
Many organizations run mixed Windows/Linux environments but struggle with identity management:
- Windows users log in with AD credentials → fine
- Linux users need separate local accounts → inefficient, insecure, no SSO
- AD permissions don't apply to Linux → access control breaks

This project fixes that by joining Linux Mint clients to a Windows AD domain using SSSD, so:
- Users log in with the same AD username/password on both OSes
- Groups and permissions are respected
- Password changes sync instantly
- Selective access is possible (e.g., some users blocked on specific clients)

## Technologies Used
- Windows Server 2019 — AD DS, DNS, DHCP
- Linux Mint — SSSD, realmd, Kerberos, PAM
- VMware Workstation — virtual lab with domain controller + clients

## What I Built / Implemented
- Configured Windows Server 2019 as domain controller (bigguy.local) with AD DS, DNS, DHCP
- Created users/groups in AD DS (shareduser, restricteduser1)
- Joined Linux Mint clients (Mint 1 & Mint 2) to the domain using realmd + SSSD
- Configured SSSD.conf for identity, authentication, access filtering, home dir mapping, and selective user allow-list
- Edited lightdm.conf for manual login and no guest access
- Joined Windows 10 client to the domain and disabled cached logons via registry
- Verified SSO logins, Kerberos tickets (klist), password sync, and selective access (restricteduser1 denied on Mint 2)
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
- DNS/time sync issues → Configured DNS and NTP to point to domain controller
- Realm join failures → Fixed resolv.conf, firewall, and package dependencies
- Selective access on Mint 2 → Used simple_allow_users in SSSD.conf
- Cached logons on Windows 10 → Disabled via registry (CachedLogonsCount = 0)

## Repository Structure
- `docs/` → Problem statement, setup guide, configs, proposal PDF
- `README.md` → Project overview and documentation

This repo demonstrates my ability to integrate Windows and Linux systems, configure AD DS, set up SSSD for domain authentication, handle Kerberos and LDAP, troubleshoot hybrid environments, and document complex projects clearly — skills directly relevant to junior systems/network admin or infrastructure roles.
