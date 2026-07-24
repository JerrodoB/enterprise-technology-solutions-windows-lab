# Naming Conventions

Enterprise Technology Solutions Windows Lab

Version: 1.0

---

## Purpose

This document defines the naming conventions used throughout the **Enterprise Technology Solutions Windows Lab**.

Consistent naming improves:

- Administrative clarity
- Searchability
- Troubleshooting
- Automation
- Security management
- Documentation quality
- Scalability

Names should communicate the purpose, type, and organizational role of an object whenever practical.

---

## General Principles

Names should be:

- Clear
- Predictable
- Consistent
- Easy to understand
- Easy to search
- Suitable for automation
- Free of unnecessary special characters

Avoid names that are:

- Ambiguous
- Overly long
- Based on temporary information
- Dependent on a single administrator’s personal preferences
- Difficult to interpret without additional explanation

---

## Active Directory Domain

Current lab domain:

```text
corp.enterpriseit.local
```

The domain name reflects a fictional corporate environment used only for training.

Production environments should use an approved enterprise namespace aligned with organizational DNS and identity strategy.

---

## Server Naming Convention

Servers should use a structured naming format.

Recommended pattern:

```text
<location>-<role>-<number>
```

Example:

```text
SD-DC-01
SD-FS-01
SD-APP-01
```

Where:

| Segment | Meaning |
|---|---|
| `SD` | San Diego location |
| `DC` | Domain Controller |
| `FS` | File Server |
| `APP` | Application Server |
| `01` | Sequential server number |

The current server is named:

```text
SERVER01
```

This name will remain in use for the existing lab. Future systems should follow the structured convention where practical.

---

## Common Server Role Codes

| Code | Role |
|---|---|
| `DC` | Domain Controller |
| `DNS` | DNS Server |
| `DHCP` | DHCP Server |
| `FS` | File Server |
| `APP` | Application Server |
| `DB` | Database Server |
| `WEB` | Web Server |
| `MGMT` | Management Server |
| `BKUP` | Backup Server |
| `MON` | Monitoring Server |
| `PKI` | Certificate Services Server |

---

## Workstation Naming Convention

Recommended pattern:

```text
<location>-<department>-WS<number>
```

Examples:

```text
SD-IT-WS001
SD-HR-WS001
SD-FIN-WS001
```

Where:

| Segment | Meaning |
|---|---|
| `SD` | San Diego location |
| `IT` | Information Technology |
| `HR` | Human Resources |
| `FIN` | Finance |
| `WS` | Workstation |
| `001` | Sequential device number |

---

## User Account Naming Convention

Standard user accounts should use a predictable logon format.

Recommended pattern:

```text
first initial + last name
```

Example:

```text
jsmith
```

If the name already exists, use an approved variation.

Examples:

```text
jsmith2
josmith
john.smith
```

The selected standard should be documented and applied consistently across the environment.

---

## Administrative Account Naming Convention

Administrative accounts should be separate from standard user accounts.

Recommended pattern:

```text
adm-<username>
```

Example:

```text
adm-jsmith
```

This separation supports:

- Least privilege
- Account auditing
- Administrative accountability
- Reduced exposure of privileged credentials

Administrators should use standard accounts for normal work and privileged accounts only when administrative access is required.

---

## Service Account Naming Convention

Recommended pattern:

```text
svc-<application-or-service>
```

Examples:

```text
svc-backup
svc-monitoring
svc-webapp
svc-sql
```

Service account names should clearly identify the system or service they support.

Service accounts should not be named after individual employees.

---

## Test Account Naming Convention

Recommended pattern:

```text
test-<purpose>
```

Examples:

```text
test-employee
test-contractor
test-gpo
test-fileshare
```

Test accounts should be clearly distinguishable from production-style user accounts.

---

## Security Group Naming Convention

Security groups should identify both their scope and purpose.

### Global Security Groups

Recommended prefix:

```text
GG_
```

Examples:

```text
GG_Employees
GG_IT
GG_HR
GG_Finance
GG_Executives
GG_Contractors
```

The prefix identifies the group as a Global Group.

---

### Domain Local Security Groups

Recommended prefix:

```text
DL_
```

Recommended pattern:

```text
DL_<resource>_<permission>
```

Examples:

```text
DL_FinanceShare_Read
DL_FinanceShare_Modify
DL_HRShare_Read
DL_ITTools_Modify
```

These groups are intended to receive permissions on specific resources.

---

### Universal Groups

Recommended prefix:

```text
UG_
```

Examples:

```text
UG_EnterpriseAdmins
UG_GlobalApplicationUsers
```

Universal Groups should be used only when the environment requires multi-domain membership or forest-wide access design.

---

## AGDLP Naming Model

The lab will use the following naming pattern for AGDLP:

```text
Accounts
    ↓
Global Groups
    ↓
Domain Local Groups
    ↓
Permissions
```

Example:

```text
jsmith
    ↓
GG_Finance
    ↓
DL_FinanceShare_Modify
    ↓
Modify permission on Finance share
```

This model separates business role membership from resource permissions.

---

## Organizational Unit Naming Convention

Organizational Units should use clear business or administrative names.

Examples:

```text
Users
Groups
Servers
Workstations
Service Accounts
Administration
```

Sub-OUs should reflect organizational or lifecycle categories.

Examples:

```text
Employees
Contractors
Executives
Test Accounts
```

Avoid unnecessary abbreviations unless they are widely understood.

---

## Shared Folder Naming Convention

Shared folders should use clear resource names.

Examples:

```text
Finance
Human Resources
IT Operations
Engineering
Public
Department Shared
```

Avoid vague names such as:

```text
Data
Files
Shared
Misc
```

unless the purpose is clearly documented.

---

## PowerShell Script Naming Convention

PowerShell scripts should use approved verbs and descriptive nouns.

Recommended pattern:

```text
Verb-Noun.ps1
```

Examples:

```text
New-ADUserBatch.ps1
Get-ServerInventory.ps1
Set-GroupMembership.ps1
Test-DNSResolution.ps1
Export-ADUserReport.ps1
```

Avoid vague filenames such as:

```text
script1.ps1
test.ps1
newscript.ps1
```

---

## Documentation File Naming Convention

Markdown files should use lowercase letters with hyphens.

Recommended pattern:

```text
lowercase-with-hyphens.md
```

Examples:

```text
documentation-standards.md
naming-conventions.md
active-directory-ou-design.md
user-account-administration.md
security-groups-and-rbac.md
```

Avoid:

```text
Documentation Standards.md
documentation_standards.md
Doc1.md
new-document.md
```

---

## Screenshot Naming Convention

Screenshots should use a numbered, descriptive format.

Recommended pattern:

```text
<number>-<description>.png
```

Examples:

```text
01-users-ou.png
02-groups-ou.png
03-new-user-wizard.png
04-get-aduser-detailed-output.png
05-group-membership-validation.png
```

The number preserves logical order.

The description explains what the screenshot demonstrates.

---

## Diagram Naming Convention

Recommended pattern:

```text
<topic>-diagram.<extension>
```

Examples:

```text
active-directory-ou-structure.png
agdpl-access-model.png
file-server-architecture.drawio
identity-lifecycle-diagram.png
```

---

## Report Naming Convention

Recommended pattern:

```text
<report-type>-<scope>-<date>.<extension>
```

Examples:

```text
user-inventory-domain-2026-07-23.csv
server-assessment-server01-2026-07-23.md
group-membership-audit-2026-07-23.csv
```

Use the ISO date format:

```text
YYYY-MM-DD
```

This format sorts correctly and avoids regional ambiguity.

---

## Git Branch Naming Convention

When branches are introduced, use a descriptive category and topic.

Recommended patterns:

```text
feature/<topic>
docs/<topic>
fix/<topic>
lab/<topic>
```

Examples:

```text
feature/file-server
docs/naming-conventions
fix/ou-documentation
lab/group-policy
```

---

## Git Commit Message Convention

Commit messages should use an imperative, descriptive format.

Examples:

```text
Add enterprise README
Document naming conventions
Implement Active Directory OU structure
Add security group screenshots
Update user administration guide
```

Avoid:

```text
Updated files
Changes
Work
Fix stuff
Final version
```

A commit message should clearly explain what the commit does.

---

## Abbreviation Standards

Use abbreviations only when they are widely recognized or formally documented.

Approved examples:

| Abbreviation | Meaning |
|---|---|
| AD | Active Directory |
| AD DS | Active Directory Domain Services |
| DNS | Domain Name System |
| DHCP | Dynamic Host Configuration Protocol |
| GPO | Group Policy Object |
| OU | Organizational Unit |
| RBAC | Role-Based Access Control |
| AGDLP | Accounts, Global Groups, Domain Local Groups, Permissions |
| NTFS | New Technology File System |
| VM | Virtual Machine |

Define less common abbreviations the first time they appear in a document.

---

## Review Checklist

Before creating or renaming an object, verify:

- The name clearly identifies its purpose.
- The correct naming pattern is being used.
- The name is consistent with existing objects.
- The name supports future automation.
- The name does not expose confidential information.
- The name does not depend on temporary conditions.
- The name is documented where necessary.

---

## Long-Term Goal

The naming standards in this repository should support an environment that is:

- Understandable
- Scalable
- Auditable
- Automatable
- Maintainable
- Professionally documented

Consistent naming is a foundational systems administration practice and should be treated as part of infrastructure design rather than as a cosmetic preference.