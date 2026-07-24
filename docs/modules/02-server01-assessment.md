# SERVER01 Infrastructure Assessment

## Assessment Purpose

This assessment documents the inherited SERVER01 virtual machine before additional configuration changes are made.

## Host Environment

| Component | Configuration |
|---|---|
| Host operating system | Windows 11 Home |
| Processor | Intel Core Ultra 7 155U |
| Installed memory | 16 GB |
| Hypervisor | VMware Workstation 17 Pro |
| VMware version | 17.6.4 |
| Virtualization | Enabled |

## Virtual Machine

| Component | Configuration |
|---|---|
| VMware virtual machine name | SERVER01 |
| Guest operating system | Windows Server 2025 Standard Evaluation |
| Assigned memory | 8 GB |
| Assigned processors | 8 virtual processors |
| Virtual disk | 60 GB |
| Network mode | Bridged |
| Server IP address | 192.168.1.201 |

## Active Directory

| Item | Finding |
|---|---|
| Active Directory domain | corp.enterpriseit.local |
| Domain Controller | SERVER01 |
| Active Directory Domain Services | Installed |
| Custom Organizational Units | None identified |
| Existing structure | Primarily default containers and system objects |

Default visible objects include:

- Builtin
- Computers
- Domain Controllers
- ForeignSecurityPrincipals
- Managed Service Accounts
- Users

Additional objects visible through Advanced Features include:

- Infrastructure
- Keys
- LostAndFound
- NTDS Quotas
- Program Data
- System
- TPM Devices

## DNS

SERVER01 also provides DNS services for Active Directory.

Identified forward lookup zones:

- corp.enterpriseit.local
- _msdcs.corp.enterpriseit.local

Observed records include:

- server01 host record: 192.168.1.201
- router host record: 192.168.1.1
- gateway alias
- Active Directory service-location records

## Initial Assessment

The environment appears to be a clean, early-stage Active Directory lab. Active Directory Domain Services and DNS are operational, but the domain has not yet been significantly customized.

The environment is suitable for learning:

- Active Directory object management
- Organizational Unit design
- User and group administration
- DNS administration
- Group Policy
- Domain troubleshooting

## Initial Risks and Constraints

- SERVER01 currently performs both Domain Controller and DNS roles.
- The virtual machine uses bridged networking and shares the physical network.
- The host system has limited memory for running multiple virtual machines.
- The domain was created during a previous course, so all prior configuration should be reviewed before making changes.
- Windows Server is an evaluation edition and has a limited evaluation period.

## Recommendation

Retain the current environment temporarily for assessment and administration practice.

Do not make major structural changes until the proposed Active Directory design has been documented and reviewed.