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


## Entry 003 — Enterprise DNS and DHCP Administration

| Field       | Details                                            |
| ----------- | -------------------------------------------------- |
| Date        | August 22, 2026                                    |
| Module      | Module 12 — Enterprise DNS and DHCP Administration |
| System      | `SERVER01` and `CLIENT01`                          |
| Domain      | `corp.enterpriseit.local`                          |
| Change type | DNS and DHCP infrastructure implementation         |
| Status      | Completed and validated                            |

### Business Purpose

Implement centrally managed enterprise DNS and DHCP services for the Enterprise Technology Solutions Windows lab while preserving the existing Active Directory, VMware NAT, and Windows file-services infrastructure.

The implementation needed to provide reliable forward and reverse name resolution, centralized client addressing, controlled DHCP allocation, DHCP/DNS integration, predictable client addressing, stale-record management, and a safe transition from VMware DHCP to Windows Server DHCP.

### Administrative Work Performed

* Assessed the existing VMnet8 NAT network before modifying DHCP services.
* Confirmed the active network as `192.168.150.0/24`.
* Confirmed the VMware host adapter at `192.168.150.1`.
* Confirmed the VMware NAT gateway at `192.168.150.2`.
* Preserved the static `SERVER01` address of `192.168.150.10`.
* Reviewed the existing Active Directory-integrated DNS configuration.
* Created the Active Directory-integrated reverse lookup zone `150.168.192.in-addr.arpa`.
* Established PTR records for `SERVER01` and `CLIENT01`.
* Created the CNAME alias `files.corp.enterpriseit.local` targeting `server01.corp.enterpriseit.local`.
* Configured DNS aging on the current reverse lookup zone using seven-day no-refresh and refresh intervals.
* Configured automatic DNS scavenging on `SERVER01` with a seven-day scavenging period.
* Installed the Windows Server DHCP role.
* Completed DHCP post-installation configuration and Active Directory authorization.
* Created the `ETS-Client-Network` DHCP scope.
* Configured the scope range `192.168.150.100–192.168.150.199`.
* Configured the exclusion range `192.168.150.100–192.168.150.127`.
* Retained an eight-day DHCP lease duration.
* Configured DHCP Option 003 — Router as `192.168.150.2`.
* Configured DHCP Option 006 — DNS Servers as `192.168.150.10`.
* Configured DHCP Option 015 — DNS Domain Name as `corp.enterpriseit.local`.
* Disabled VMware DHCP before activating the Windows DHCP scope.
* Preserved VMware NAT while transferring DHCP responsibility to `SERVER01`.
* Released the previous VMware-issued lease on `CLIENT01` and obtained a Windows DHCP lease.
* Created a DHCP reservation for `CLIENT01` at `192.168.150.128`.
* Configured DHCP/DNS dynamic-update integration.
* Renewed the CLIENT01 lease and confirmed that the DHCP reservation became active.
* Removed obsolete `gateway` and `router` DNS records associated with the former `192.168.1.0/24` network.
* Removed the obsolete `1.168.192.in-addr.arpa` reverse lookup zone after replacement DNS infrastructure was validated.

### Validation Performed

* Confirmed forward DNS resolution for `SERVER01`.
* Confirmed forward DNS resolution for `CLIENT01`.
* Confirmed reverse DNS resolution for `192.168.150.10`.
* Confirmed reverse DNS resolution for `192.168.150.128`.
* Confirmed that `files.corp.enterpriseit.local` resolved through its CNAME target.
* Confirmed that `SERVER01` passed Active Directory and DNS health checks.
* Confirmed that `CLIENT01` maintained a valid Active Directory secure channel.
* Confirmed that VMware DHCP was disabled before Windows DHCP scope activation.
* Confirmed that `CLIENT01` received its network configuration from DHCP server `192.168.150.10`.
* Confirmed that the client received gateway `192.168.150.2`.
* Confirmed that the client received DNS server `192.168.150.10`.
* Confirmed that the client received the `corp.enterpriseit.local` DNS domain configuration.
* Confirmed that the CLIENT01 DHCP reservation became active.
* Confirmed that `\\SERVER01\HR` remained accessible following the DHCP migration.
* Confirmed that the final DNS configuration no longer contained the obsolete `192.168.1.0/24` records and reverse zone.

### Operational Risks and Controls

| Operational risk                                 | Control applied                                                               |
| ------------------------------------------------ | ----------------------------------------------------------------------------- |
| Competing DHCP servers on VMnet8                 | VMware DHCP disabled before Windows DHCP scope activation                     |
| Loss of external connectivity                    | VMware NAT retained and gateway `192.168.150.2` preserved                     |
| Incorrect addressing of infrastructure services  | `SERVER01` retained static address `192.168.150.10`                           |
| Domain clients receiving an incorrect DNS server | DHCP Option 006 configured as `192.168.150.10`                                |
| Domain clients receiving an incorrect gateway    | DHCP Option 003 configured as `192.168.150.2`                                 |
| Dynamic allocation of protected addresses        | DHCP exclusion range configured                                               |
| Unpredictable CLIENT01 addressing                | DHCP reservation created for `192.168.150.128`                                |
| Missing reverse name resolution                  | Current `192.168.150.0/24` reverse lookup zone implemented                    |
| Accumulation of stale dynamic DNS records        | DNS aging and scavenging configured                                           |
| Unauthorized Windows DHCP service                | DHCP server authorized in Active Directory                                    |
| Inconsistent DHCP and DNS records                | DHCP/DNS dynamic-update integration configured                                |
| Obsolete DNS information from former network     | Legacy forward records and reverse lookup zone removed after validation       |
| Disruption of earlier module services            | Active Directory secure channel and HR share access validated after migration |

### Troubleshooting and Resolution

During reverse-DNS implementation, duplicate PTR records for `SERVER01` appeared after a manual PTR was created while a dynamically registered PTR was already present. The manually created static duplicate was removed, leaving the dynamically registered record.

During initial reverse-resolution investigation, a previously cached PTR response produced a result that differed from the authoritative DNS server state. The DNS client cache was inspected and the cached record subsequently expired, confirming the difference between client-side cached DNS information and authoritative DNS zone data.

After the DHCP reservation for `CLIENT01` was created, DHCP Manager initially displayed the reservation as inactive because the client was still operating with the lease issued before the reservation existed. `CLIENT01` released and renewed its lease, allowing `SERVER01` to match the client MAC address to the reservation. The reservation then became active.

### Outcome

Enterprise DNS and DHCP services are active on `SERVER01`.

The lab now provides Active Directory-integrated forward and reverse DNS, service-oriented DNS aliasing, DNS aging and scavenging, Active Directory-authorized Windows DHCP, centralized scope options, controlled dynamic addressing, an active CLIENT01 reservation, and DHCP/DNS integration.

DHCP responsibility was successfully migrated from VMware to Windows Server without disrupting VMware NAT, Active Directory domain connectivity, DNS resolution, or the existing HR file service.

Legacy DNS configuration associated with the former `192.168.1.0/24` network was removed after the replacement `192.168.150.0/24` infrastructure was validated.

### Supporting Documentation

* [Enterprise DNS and DHCP Administration](../modules/12-enterprise-dns-dhcp-administration.md)

### Validation Evidence

* [Reverse DNS configuration](../../screenshots/module-12-enterprise-dns-dhcp-administration/01-reverse-dns-configuration.png)
* [DNS aging and scavenging](../../screenshots/module-12-enterprise-dns-dhcp-administration/03-dns-aging-scavenging.png)
* [DHCP post-installation authorization](../../screenshots/module-12-enterprise-dns-dhcp-administration/05-dhcp-post-install-authorization.png)
* [VMware DHCP disabled](../../screenshots/module-12-enterprise-dns-dhcp-administration/07-vmware-dhcp-disabled.png)
* [Windows DHCP scope active](../../screenshots/module-12-enterprise-dns-dhcp-administration/08-windows-dhcp-scope-active.png)
* [CLIENT01 Windows DHCP lease](../../screenshots/module-12-enterprise-dns-dhcp-administration/09-client01-windows-dhcp-lease.png)
* [DHCP/DNS integration](../../screenshots/module-12-enterprise-dns-dhcp-administration/11-dhcp-dns-integration.png)
* [DHCP scope options](../../screenshots/module-12-enterprise-dns-dhcp-administration/12-dhcp-scope-options.png)
* [Final DNS configuration](../../screenshots/module-12-enterprise-dns-dhcp-administration/13-final-dns-configuration.png)
* [Active CLIENT01 DHCP reservation](../../screenshots/module-12-enterprise-dns-dhcp-administration/14-client01-active-dhcp-reservation.png)
