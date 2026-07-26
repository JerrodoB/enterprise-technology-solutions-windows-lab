# Professional Operations Log

## Purpose

This log provides a chronological record of significant administrative work completed in the Enterprise Technology Solutions Windows Lab.

Each entry documents the business purpose, implementation activities, operational risks, validation methods, results, and supporting evidence associated with a meaningful unit of work.

---

## Entry 001 — HR Share AGDLP Access Control

| Field | Details |
|---|---|
| Date | July 25, 2026 |
| Module | Module 9 — Enterprise Identity Administration |
| System | `SERVER01` |
| Domain | `corp.enterpriseit.local` |
| Change type | Identity and access-management implementation |
| Status | Completed and validated |

### Business Purpose

Implement role-based access to a Human Resources file share while protecting sensitive business information from unauthorized users.

The access design needed to support least privilege, simplified employee-access administration, and separation between business-role membership and resource permissions.

### Administrative Work Performed

- Created the Domain Local security group `DL_HR_Modify`.
- Used the Global security group `GG_HR` to represent the Human Resources business role.
- Nested `GG_HR` inside `DL_HR_Modify`.
- Assigned John Smith to `GG_HR` as an authorized HR employee.
- Left Jane Doe outside `GG_HR` to support unauthorized-access testing.
- Assigned SMB `Change` access to `CORP\DL_HR_Modify`.
- Assigned explicit NTFS `Modify` access to `CORP\DL_HR_Modify`.
- Removed the broad `Everyone` entry from the SMB share.
- Avoided direct permission assignments to individual user accounts.

The implemented access chain was:

```text
john.smith
    ↓
GG_HR
    ↓
DL_HR_Modify
    ↓
SMB Change and NTFS Modify
    ↓
\\SERVER01\HR
```

### Validation Performed

- Confirmed that John Smith was a member of `GG_HR`.
- Confirmed that `GG_HR` was a member of `DL_HR_Modify`.
- Verified the SMB and NTFS permission assignments independently.
- Confirmed that John Smith could create, read, modify, and delete a test file.
- Confirmed that Jane Doe could not browse the share or create a file.
- Verified that Jane Doe’s failed write attempt left no unauthorized file behind.
- Restored **User must change password at next logon** for both test accounts.
- Confirmed the restored password settings with a directory-side LDAP query.

### Operational Risks and Controls

| Operational risk | Control applied |
|---|---|
| Unauthorized access to HR information | Role-based group membership and negative testing |
| Excessive access rights | Change and Modify used instead of Full Control |
| Direct permissions assigned to users | AGDLP group nesting used |
| Broad default share access | `Everyone` removed from the SMB share |
| Misaligned SMB and NTFS permissions | Both permission layers independently validated |
| Temporary testing exceptions remaining active | Password-change requirements restored and verified |

### Outcome

The AGDLP implementation operated as designed. Authorized access was granted through business-role membership, unauthorized access was denied, least-privilege permissions were maintained, and temporary testing changes were securely reversed.

### Supporting Documentation

- [Security Groups, RBAC, and AGDLP Access Control](../modules/05-security-groups-and-rbac.md)

### Validation Evidence

- [AGDLP group-nesting validation](../../screenshots/module-09-enterprise-identity-administration/agdlp-access-control/01-agdlp-group-nesting-validation.png)
- [SMB and NTFS permission validation](../../screenshots/module-09-enterprise-identity-administration/agdlp-access-control/02-agdlp-share-and-ntfs-permissions-validation.png)