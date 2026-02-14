# Problem Statement

**Project Context**  
This solution was developed for a simulated enterprise scenario (BigGuy Corp) requiring reliable multi-login authentication in a mixed Windows and Linux environment. The original assignment involved a group, but I personally designed, implemented, configured, and documented the complete architecture.

**Core Requirements**  
- Enable centralized user authentication for all clients (Windows and Linux)
- Allow seamless logins from Linux Mint clients using Active Directory domain credentials
- Support user and group management from a single point (Windows Server 2019 AD DS)
- Provide secure authentication (Kerberos) and identity lookup (LDAP)
- Map permissions and access controls (NTFS/share permissions to Linux clients)
- Demonstrate functional integration in a virtual lab environment

**Key Challenge**  
Linux clients lacked native support for Windows Active Directory domain authentication without complex manual configuration or additional software. A practical, scalable integration was needed to allow Linux users to log in with AD credentials and access domain resources.

**Objective**  
Deliver a reliable multi-login authentication system using Active Directory Domain Services (AD DS) on Windows Server 2019 as the domain controller, with SSSD on Linux Mint clients for domain join, Kerberos authentication, and LDAP identity lookup. The solution provides centralized identity management, secure logins, and permission mapping in a hybrid environment.
