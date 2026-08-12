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


---

## Entry 002 — Enterprise HR File Services Administration

| Field | Details |
|---|---|
| Date | August 12, 2026 |
| Module | Module 11 — Enterprise Windows File Services Administration |
| System | `SERVER01` |
| Domain | `corp.enterpriseit.local` |
| Change type | File-service security and storage-governance implementation |
| Status | Completed and validated |

### Business Purpose

Strengthen the Human Resources file service by implementing protected network communication, controlled resource visibility, restricted-folder authorization, capacity enforcement, prohibited-file controls, and auditable operational notifications.

The implementation needed to preserve the existing AGDLP authorization model while improving the security, governance, maintainability, and monitoring of departmental file storage.

### Administrative Work Performed

- Retained the existing AGDLP authorization chain for the `HR` share.
- Confirmed SMB `Change` and NTFS `Modify` permissions for `CORP\DL_HR_Modify`.
- Enabled SMB encryption on the `HR` share.
- Enabled Access-Based Enumeration.
- Implemented a restricted `Confidential` subfolder using:
  - `GG_HR_Confidential`
  - `DL_HR_Confidential_Modify`
- Installed File Server Resource Manager.
- Created the quota template `ETS Department Share - 100 MB Hard Limit`.
- Applied a 100 MB hard quota to `C:\Shares\HR`.
- Configured an 85% Event Log warning threshold.
- Disabled unconfigured email-notification actions.
- Created the active file-screen template `ETS Department Share - Block Executable Files`.
- Applied the built-in `Executable Files` file group to `C:\Shares\HR`.
- Restored the global Event Log notification interval to 60 minutes after controlled validation.
- Removed all temporary quota and file-screen test files.

### Validation Performed

- Confirmed SMB encryption was enabled.
- Confirmed the folder-enumeration mode was `AccessBased`.
- Verified the SMB access assigned to `CORP\DL_HR_Modify`.
- Validated authorized and unauthorized access behavior.
- Confirmed the restricted-folder authorization model.
- Crossed the 85% quota threshold and validated `SRMSVC` Event ID `12325`.
- Verified that no new email-notification error occurred after correcting the active quota.
- Confirmed that the 100 MB hard quota rejected a write that would have exceeded the limit.
- Confirmed that no residual file remained after the rejected write.
- Attempted an `.exe` file write and confirmed that active screening blocked it.
- Validated `SRMSVC` Event ID `8215` for the file-screen violation.
- Created and removed a permitted `.txt` file to confirm that ordinary document types remained allowed.
- Verified that the quota and file screen remained enabled and matched their approved templates.
- Confirmed that quota usage returned to its normal baseline after cleanup.

### Operational Risks and Controls

| Operational risk | Control applied |
|---|---|
| Unauthorized access to HR information | AGDLP authorization and layered SMB/NTFS permissions |
| Exposure of inaccessible folder names | Access-Based Enumeration |
| Interception of SMB traffic | SMB encryption |
| Excessive access to confidential information | Dedicated Global and Domain Local groups for the restricted folder |
| Uncontrolled storage growth | 100 MB hard quota |
| Lack of early capacity warning | 85% Event Log notification threshold |
| Storage of executable files | Active file screen using the `Executable Files` group |
| Failed notifications caused by missing SMTP configuration | Email actions disabled; Event Log notification retained |
| Repeated notification suppression during validation | Global notification interval identified, temporarily adjusted, and restored |
| Temporary test artifacts remaining in the share | Test files removed and final usage validated |
| Template and deployed-object configuration drift | Active quota and file screen audited directly |

### Troubleshooting and Resolution

The quota template was corrected to remove email notification, but the active quota continued attempting to send email because it retained its previously assigned action. Email was therefore disabled directly on the quota applied to `C:\Shares\HR`.

A subsequent threshold retest did not immediately produce another Event Log warning. Investigation determined that the global File Server Resource Manager Event Log notification limit was set to 60 minutes. The interval was temporarily reduced to one minute for controlled validation and restored to 60 minutes after Event ID `12325` was successfully generated.

### Outcome

The HR share now operates with layered authorization, encrypted SMB traffic, reduced resource disclosure, separate confidential-folder access, enforced storage capacity, prohibited executable-file controls, and Event Log monitoring.

All enforcement and permitted-use tests passed. Temporary files and testing adjustments were removed, and the final configuration remains active and aligned with its approved templates.

### Supporting Documentation

- [Windows File Services Administration](../modules/07-windows-file-services-administration.md)

### Validation Evidence

- [FSRM quota-threshold event](../../screenshots/module-11-enterprise-windows-file-services-administration/01-fsrm-quota-threshold-event.png)
- [File-screen violation event](../../screenshots/module-11-enterprise-windows-file-services-administration/02-file-screen-violation-event.png)
- [SMB share security baseline](../../screenshots/module-11-enterprise-windows-file-services-administration/03-smb-share-security-baseline.png)
- [FSRM configuration baseline](../../screenshots/module-11-enterprise-windows-file-services-administration/04-fsrm-configuration-baseline.png)