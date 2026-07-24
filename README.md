# Enterprise Technology Solutions Windows Lab

> A portfolio-driven enterprise Windows Server administration project focused on Active Directory, PowerShell, enterprise infrastructure, documentation, and operational excellence.

---

## Overview

The **Enterprise Technology Solutions Windows Lab** is a long-term professional development project designed to simulate the responsibilities of a Windows Systems Administrator working in an enterprise environment.

Rather than performing isolated lab exercises, this project follows enterprise administration practices including:

- Infrastructure design
- Active Directory administration
- Identity lifecycle management
- PowerShell automation
- Documentation
- Change validation
- Version control using Git
- Technical portfolio development

Every configuration change is documented, validated, and committed to version control.

---

## Professional Objectives

This project supports my transition from a **Business Systems Analyst** into a **Windows Systems Administrator**, while building the foundation required for future roles in:

- Windows Infrastructure Engineering
- Hybrid Infrastructure Administration
- Enterprise Infrastructure Solutions Architecture

---

## Lab Environment

| Component | Configuration |
|------------|---------------|
| Virtualization Platform | VMware Workstation |
| Operating System | Windows Server 2025 Standard Evaluation |
| Primary Server | SERVER01 |
| Active Directory Domain | `corp.enterpriseit.local` |
| Organization | Enterprise Technology Solutions |
| Directory Services | Active Directory Domain Services |
| DNS | Active Directory Integrated |
| Administration Tools | Server Manager, ADUC, PowerShell |
| Version Control | Git |
| Repository Hosting | GitHub |

---

# Enterprise Infrastructure

## Organizational Unit Structure

```text
corp.enterpriseit.local
└── Enterprise Technology Solutions
    ├── Administration
    ├── Computers
    ├── Groups
    ├── Servers
    ├── Service Accounts
    ├── Users
    │   ├── Employees
    │   ├── Contractors
    │   ├── Executives
    │   └── Test Accounts
    └── Workstations
```

The OU structure was designed to support:

- Delegated administration
- Group Policy
- Security boundaries
- Lifecycle management
- Scalability
- Enterprise operational standards

---

## Security Group Design

Current Global Security Groups:

```text
GG_Employees
GG_IT
GG_HR
GG_Finance
GG_Executives
GG_Contractors
```

The project follows Microsoft's recommended access control model.

```text
Permissions
        ↑
Domain Local Groups
        ↑
Global Groups
        ↑
User Accounts
```

Future modules will implement the complete AGDLP model.

---

# Skills Demonstrated

Current technical competencies include:

- Windows Server administration
- Active Directory administration
- Organizational Unit design
- User account provisioning
- Security Group administration
- Role-Based Access Control (RBAC)
- DNS administration
- Enterprise documentation
- PowerShell administration
- Git version control
- Infrastructure validation
- Troubleshooting methodology

---

# PowerShell

PowerShell commands introduced during the project include:

```powershell
Import-Module ActiveDirectory

Get-ADUser

New-ADUser

Set-ADUser

Enable-ADAccount

Set-ADAccountPassword

Get-ADOrganizationalUnit
```

Future modules will expand into PowerShell scripting and automation.

---

# Repository Structure

```text
enterprise-technology-solutions-windows-lab
│
├── README.md
├── CHANGELOG.md
├── CONTRIBUTING.md
├── LICENSE
├── .gitignore
│
├── docs
│   ├── modules
│   ├── architecture
│   ├── standards
│   ├── decisions
│   ├── troubleshooting
│   └── references
│
├── powershell
│   ├── active-directory
│   └── server-administration
│
├── screenshots
│
├── diagrams
│
├── reports
│
└── labs
```

---

# Documentation

The repository separates operational documentation into dedicated categories.

| Directory | Purpose |
|------------|----------|
| docs/modules | Module documentation |
| docs/architecture | Infrastructure architecture |
| docs/standards | Enterprise standards |
| docs/decisions | Technical decision records |
| docs/troubleshooting | Troubleshooting knowledge base |
| docs/references | Technical references |
| screenshots | Configuration evidence |
| diagrams | Infrastructure diagrams |
| reports | Generated reports |
| powershell | Administrative scripts |

---

# Current Progress

Completed work includes:

- Windows Server virtual lab deployment
- Enterprise Active Directory assessment
- Organizational Unit design
- Organizational Unit implementation
- Enterprise user hierarchy
- Active Directory PowerShell module
- User provisioning
- Security Group creation
- Git installation
- Git repository initialization
- Repository structure creation

---

# Current Module

The current module focuses on:

- Enterprise Identity Administration
- User lifecycle management
- Security Groups
- Role-Based Access Control
- Authentication vs Authorization
- PowerShell administration

---

# Planned Development

Future project modules include:

- AGDLP implementation
- Group Policy
- File Servers
- NTFS permissions
- Shared folders
- DFS
- DHCP
- Certificate Services
- PowerShell automation
- Backup and recovery
- Windows security hardening
- Monitoring
- Enterprise troubleshooting
- Azure hybrid integration

---

# Methodology

This project follows the enterprise administration workflow below.

```text
Design
    ↓
Implement
    ↓
Validate
    ↓
Document
    ↓
Commit
```

This methodology follows the **Small Batches Principle** described in *The Practice of System and Network Administration*.

---

# Reference Material

Primary reference:

> The Practice of System and Network Administration  
> Volume 1 – DevOps and Other Best Practices for Enterprise IT  
> Third Edition

Additional references include Microsoft Learn documentation and other enterprise infrastructure resources.

---

# Security Notice

This repository intentionally excludes:

- Passwords
- Credentials
- Private keys
- Proprietary company information
- Confidential production configurations
- Personally identifiable production data

All infrastructure contained in this repository is intended solely for educational and professional portfolio purposes.

---

# Professional Development Roadmap

```text
Windows Systems Administration
            ↓
Linux Systems Administration
            ↓
Azure Administration
            ↓
Infrastructure Engineering
            ↓
Enterprise Infrastructure Solutions Architecture
```

---

# About the Author

**Jerrodo Butler**

Business Systems Analyst, MBA Candidate, and Enterprise Infrastructure professional focused on developing expertise in:

- Enterprise Windows Administration
- Infrastructure Engineering
- Systems Thinking
- Enterprise Architecture
- Technical Documentation
- Technology Strategy

---

## Repository Status

**Current Status**

🟢 Active Development

Project modules, documentation, PowerShell automation, and enterprise infrastructure implementations are continuously being expanded as part of the Enterprise Systems Administration Development Program (ESDP).