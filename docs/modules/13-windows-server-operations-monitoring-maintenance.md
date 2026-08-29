# Module 13 — Enterprise Windows Server Operations, Monitoring, and Maintenance

## Overview

Module 13 transitioned the Enterprise Systems Administration Development Program from primarily deploying Windows infrastructure services to operating, monitoring, maintaining, and evaluating an established Windows Server environment.

The existing SERVER01 infrastructure was used as the operational workload. The module focused on determining whether server roles, Windows services, operating-system resources, storage, and maintenance state were healthy without introducing unnecessary infrastructure or artificial failures.

## Environment

### Domain

- `corp.enterpriseit.local`

### SERVER01

- Windows Server 2025
- IPv4: `192.168.150.10`
- Active Directory Domain Services
- DNS Server
- DHCP Server
- Windows File Services
- File Server Resource Manager (FSRM)

### CLIENT01

- Windows 11 domain workstation
- DHCP reservation: `192.168.150.128`
- DNS server: `192.168.150.10`
- Default gateway: `192.168.150.2`

### VMware Network

- VMware Workstation VMnet8 NAT
- Network: `192.168.150.0/24`
- VMware NAT gateway: `192.168.150.2`
- VMware DHCP disabled
- Windows Server DHCP authoritative for the lab

## Operational Monitoring

### Server Manager

Server Manager was used to establish a high-level operational view of SERVER01 and its installed roles.

The assessment included:

- Server manageability
- Installed server roles
- Service status
- Event indicators
- Performance-monitoring availability
- Local Server configuration and operational properties

The exercise reinforced that high-level warnings should be investigated in context rather than treated automatically as evidence of server failure.

## Windows Services Administration

The Windows Services console was used to examine the services supporting the existing infrastructure.

Critical services included:

- Active Directory Domain Services (`NTDS`)
- DNS Server (`DNS`)
- DHCP Server (`DHCPServer`)
- Server/SMB service (`LanmanServer`)
- File Server Resource Manager (`SrmSvc`)

Service properties were reviewed to understand:

- Service status
- Startup behavior
- Service identity
- Executable path
- Dependencies
- Recovery behavior

Service dependencies were evaluated before performing maintenance, reinforcing the importance of understanding downstream operational impact before stopping or restarting services.

Recovery configuration was also reviewed to distinguish automatic service recovery from root-cause investigation.

## Event Monitoring

Windows Event Viewer was used to review the Application and System logs and to distinguish:

- Information
- Warning
- Error
- Critical

The System log was filtered to reduce event volume and focus on operationally significant events.

Event investigation emphasized correlation between:

- Log
- Event level
- Timestamp
- Source
- Event ID
- General description
- Current service state

### FSRM Event Correlation

A File Server Resource Manager warning was identified in the Application log:

- Source: `SRMSVC`
- Event ID: `12317`
- Level: Warning
- Computer: `SERVER01.corp.enterpriseit.local`

The event reported that File Server Resource Manager had failed to enumerate share or DFS paths and would retry the operation later.

The warning was correlated with the current state of the File Server Resource Manager service, which was confirmed to be running.

No corrective action was taken because the historical warning alone did not establish a current FSRM outage.

This demonstrated the operational principle:

`Observe → Correlate → Interpret → Decide → Act`

## Real-Time Resource Monitoring

### Task Manager

Task Manager was used to establish a high-level real-time view of:

- CPU
- Memory
- Disk
- Network

The exercise distinguished a current utilization snapshot from a performance baseline collected over time.

### Resource Monitor

Resource Monitor was used to examine resource utilization at greater detail, including:

- CPU activity by process
- Memory utilization
- Disk activity
- Network activity
- TCP connections
- Listening ports

Task Manager and Resource Monitor were treated as complementary operational tools rather than substitutes for structured performance monitoring.

## Performance Monitoring

Windows Performance Monitor was used to select meaningful counters based on specific operational questions.

The following counter set was established:

| Resource | Performance Counter |
| --- | --- |
| CPU | `Processor\% Processor Time\_Total` |
| Memory | `Memory\Available MBytes` |
| Disk utilization | `PhysicalDisk\% Disk Time\_Total` |
| Disk latency | `PhysicalDisk\Avg. Disk sec/Transfer\_Total` |
| Network | `Network Interface\Bytes Total/sec` |

The network counter monitored the active VMware-presented Intel 82574L Gigabit Network Connection.

## SERVER01 Performance Baseline

A custom Data Collector Set named:

`SERVER01 Performance Baseline`

was created.

Configuration included:

- Five selected performance counters
- 15-second sampling interval
- Normal SERVER01 lab workload
- No artificial CPU, memory, disk, or network load

A recorded baseline was collected for approximately 12.5 minutes.

### Baseline Results

| Resource | Counter | Average |
| --- | --- | ---: |
| CPU | `% Processor Time` | 1.546% |
| Memory | `Available MBytes` | 5,593.373 MB |
| Disk utilization | `% Disk Time` | 0.203% |
| Disk latency | `Avg. Disk sec/Transfer` | approximately 0.000 sec |
| Network | `Bytes Total/sec` | 243.903 bytes/sec |

### Baseline Assessment

SERVER01 operated under a very light workload during the collection period.

The collected data showed no evidence of CPU, memory, disk, or network resource pressure during the measured interval.

The baseline is an environment-specific reference for SERVER01 under normal light ESDP lab operation. It is not intended to establish universal Windows Server performance thresholds or production capacity requirements.

## Storage Capacity and Health

The SERVER01 system volume was evaluated separately from disk-performance measurements.

### C: Volume

- File system: NTFS
- Capacity: 59.0 GB
- Used space: 13.3 GB
- Free space: 45.7 GB
- Approximate free capacity: 77.5%
- Volume health: Healthy

An ambiguous warning indicator in the Server Manager volume view was investigated through the volume-specific Properties view. The C: volume reported `Healthy`, and no storage repair or configuration change was required.

This reinforced the importance of validating high-level warnings against resource-specific evidence before taking corrective action.

## PowerShell Operational Administration

PowerShell was used as a read-only operational verification tool.

The following command provided a consolidated health check of critical infrastructure services:

```powershell
Get-Service NTDS,DNS,DHCPServer,LanmanServer,SrmSvc