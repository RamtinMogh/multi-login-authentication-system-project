# Problem Statement

**Project Context**  
This solution was developed for a simulated enterprise scenario (BigGuy Corp) requiring a multi-login authentication system in a mixed Windows and Linux environment. The original assignment involved a group, but I personally designed, implemented, configured, and documented the complete architecture.

**Core Requirements**  
- Enable centralized authentication for all users across Windows and Linux clients
- Allow seamless logins from Linux Mint clients using Active Directory credentials
- Support user/group management from a single point (Windows Server 2019 AD DS)
- Provide secure authentication (Kerberos) and identity lookup (LDAP)
- Map permissions and access controls (NTFS/share permissions to Linux)
- Use open-source tools on Linux side (SSSD, realmd, Kerberos) with no additional purchases

**Key Challenge**  
Linux clients could not natively authenticate to Windows AD domain without complex manual configuration or proprietary solutions. A practical, scalable integration was needed using SSSD for domain join and authentication.

**Objective**  
Deliver a reliable, centralized multi-login system using AD DS on Windows Server 2019 as the domain controller and SSSD on Linux Mint clients — all open-source on the Linux side, cost-free, and fully documented in a virtual lab environment.
