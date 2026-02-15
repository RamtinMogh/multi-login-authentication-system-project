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

Configure /etc/sssd/sssd.conf:
```ini
[sssd]
domains = bigguy.local
config_file_version = 2
services = nss, pam, pac

[domain/bigguy.local]
ad_domain = bigguy.local
krb5_realm = BIGGUY.LOCAL
realmd_tags = manages-system joined-with-adcli
id_provider = ad
access_provider = simple
ad_access_filter = (memberOf=CN=Domain Users,CN=Users,DC=bigguy,DC=local)
cache_credentials = True
default_shell = /bin/bash
fallback_homedir = /home/%u@%d
use_fully_qualified_names = True
ldap_id_mapping = True
krb5_store_password_if_offline = True
simple_allow_users = shareduser@bigguy.local, restricteduser1@bigguy.local
debug_level = 9
```

Save and exit the editor.

Note: For Linux Mint 2 (selective access), do not include restricteduser1@bigguy.local in simple_allow_users.

Edit /etc/lightdm/lightdm.conf:
```ini
[Seat:*]
greeter-session=lightdm-gtk-greeter
user-session=linuxmint
greeter-show-manual-login=true
allow-guest=false
allow-user-switching=true
```

Restart services:
```bash
sudo systemctl restart sssd
sudo systemctl restart lightdm
```

Test login with shareduser@bigguy.local

Confirm AD account:
```bash
klist
whoami
```

Test password change: Change in ADUC on DC, verify on clients.


## Step 3: Windows 10 Client Setup
Set to Host-only networking:
- IP address: 192.168.100.20
- Subnet Mask: 255.255.255.0
- Default Gateway: 192.168.100.10
- DNS: 192.168.100.10

System properties → Computer name → Change → Select domain: bigguy.local
Provide Administrator credentials

Restart to apply changes

Login with BIGGUY\shareduser

Disable cached credentials (in admin command prompt or PowerShell):

```powershell
reg add "HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Winlogon" /v CachedLogonsCount /t REG_SZ /d 0 /f
```


## Verification Steps
- AD users can log in to Linux Mint and Windows 10 with domain credentials
- Groups and selective access work (e.g., restricteduser1 denied on Mint 2)
- Kerberos tickets issued (klist)
- Password changes sync across clients
- DNS resolving correctly on Linux (nslookup bigguy.local)

## Additional Notes
- Firewall: Allow necessary ports if needed
- Troubleshooting: Check SSSD debug logs (debug_level = 9 in sssd.conf)
- No proprietary tools used on Linux side

See full original proposal: [BigGuy Corp Multi-Login Authentication Proposal](../docs/BigGuy-Corp-Authentication-Proposal.pdf)
