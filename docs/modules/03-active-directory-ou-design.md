# Active Directory OU Design

## Design Goals

The Active Directory Organizational Unit (OU) structure for Enterprise Technology Solutions (ETS) is designed to support a secure, scalable, and easily administered enterprise environment.

The design goals are:

- Organize Active Directory according to business and administrative functions rather than individual technologies.
- Simplify day-to-day administration.
- Support future growth without requiring major restructuring.
- Enable the use of Group Policy to manage different categories of objects.
- Support delegated administration while minimizing the use of Domain Administrator privileges.
- Separate administrative accounts, user accounts, computers, servers, and service accounts.
- Follow Microsoft and enterprise Active Directory best practices.
- Produce a structure that is easy for future administrators to understand and maintain.

## Proposed OU Structure (organize by administrative function first, then by departments)

Enterprise Technology Solutions
│
├── Administration
├── Computers
├── Groups
├── Servers
├── Service Accounts
├── Users
└── Workstations

## Naming Standards

### User Accounts

- firstname.lastname

### Administrative Accounts

- adm-firstname.lastname

### Servers

- DC01
- FS01
- APP01
- SQL01
- WEB01

### Workstations

- WS-001
- WS-002
- WS-003

### Security Groups

- GG_IT_Admins
- GG_HelpDesk
- GG_Finance
- GG_Engineering

### Service Accounts

- svc-backup
- svc-sql
- svc-monitoring

## Administrative Model

Enterprise Technology Solutions (ETS) follows the Principle of Least Privilege by assigning administrative responsibilities according to job function rather than granting unrestricted administrative access.

### Administrative Roles

**Domain Administrator**
- Responsible for overall Active Directory health.
- Performs domain-wide configuration changes.
- Manages Domain Controllers.
- Creates and manages enterprise administrative policies.
- Used only when elevated privileges are required.

**Systems Administrator**
- Manages Windows Servers.
- Creates and manages Organizational Units.
- Creates user and computer accounts.
- Manages Group Policy.
- Performs routine server administration.

**Help Desk Administrator**
- Resets user passwords.
- Unlocks user accounts.
- Assists users with account-related issues.
- Does not have Domain Administrator privileges.

**Desktop Support**
- Supports workstation computers.
- Joins computers to the domain.
- Troubleshoots workstation issues.
- Does not manage Active Directory infrastructure.

### Administrative Principle

Administrative permissions should always be assigned according to operational responsibilities and the Principle of Least Privilege.



## Future Group Policy Considerations

The Organizational Unit structure is designed to support future Group Policy Objects (GPOs) that will be implemented throughout the Enterprise Technology Solutions (ETS) environment.

Planned Group Policy targets include:

### Domain Controllers

- Enhanced security settings
- Advanced auditing
- Domain Controller hardening

### Servers

- Windows Update configuration
- Security baselines
- Event log configuration
- Remote administration settings

### Workstations

- Password and lock screen policies
- Desktop configuration
- Microsoft Defender settings
- Windows Update policies
- Software deployment

### Users

- Folder redirection (future)
- Desktop restrictions
- Login scripts
- Drive mappings

### Administrative Accounts

- Multi-factor authentication (future)
- Enhanced auditing
- Restricted administrative privileges

The Organizational Unit hierarchy has been designed so that Group Policies can be assigned to specific object types without affecting unrelated systems.

## Design Decisions

The Active Directory design for Enterprise Technology Solutions (ETS) is based on enterprise administration rather than organizational structure.

The following design decisions were made:

- Organizational Units are organized according to administrative function to simplify management and reduce future restructuring.
- Separate Organizational Units will be used for servers, workstations, users, groups, and service accounts to support targeted administration and Group Policy.
- Administrative responsibilities will follow the Principle of Least Privilege.
- Consistent naming standards will be used for all Active Directory objects.
- The design supports future growth without requiring significant structural changes.
- Group Policy planning has been incorporated into the Organizational Unit design rather than being added later.
- Simplicity has been prioritized over excessive Organizational Unit nesting to improve administration and troubleshooting.
- All significant infrastructure decisions will be documented before implementation.

## Implementation Plan

The Active Directory design for Enterprise Technology Solutions (ETS) will be implemented in the following sequence:

1. Validate the existing Active Directory environment.
2. Create the Organizational Unit hierarchy.
3. Configure standardized naming conventions.
4. Create security and administrative groups.
5. Create user and computer accounts.
6. Implement delegated administrative permissions.
7. Develop and apply Group Policy Objects.
8. Validate functionality and security.
9. Document the completed implementation.
10. Review the environment for future improvements.

This phased implementation approach reduces risk, supports consistent administration, and provides a structured foundation for future expansion.

## Implementation Validation

The Organizational Unit hierarchy was successfully implemented in Active Directory according to the approved design.

Validation confirmed:

- All planned Organizational Units were created.
- The hierarchy matches the documented architecture.
- Organizational Units were created in their intended locations.
- Protection against accidental deletion is enabled.
- The environment is ready for user, computer, group, and Group Policy implementation in future modules.