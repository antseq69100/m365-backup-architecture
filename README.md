# Microsoft 365 Backup Architecture Lab – Veeam Backup for Microsoft 365

## Related Projects

- m365-endpoint (Intune / Autopilot / Update Rings / Win32 apps deployment)

## Overview

This repository documents the deployment of a Microsoft 365 tenant backup solution using **Veeam Backup for Microsoft 365** in a virtualized lab environment based on **Proxmox VE**.

The objective of this project is to design a realistic backup architecture similar to production environments where:

* backup services run on a dedicated server
* storage is externalized from compute
* authentication uses Microsoft Entra ID modern authentication
* the backup server remains fully reconstructible

Protected workloads include:

* Exchange Online
* SharePoint Online
* OneDrive for Business
* Microsoft Teams

---

## Architecture

Logical backup flow:

```
Microsoft 365 Tenant
        │
        ▼
Veeam Backup for Microsoft 365 Server (Windows Server VM)
        │
        ▼
External Backup Repository (mounted via Proxmox storage)
```

Infrastructure stack:

```
Hypervisor: Proxmox VE
Backup Server OS: Windows Server 2022 Evaluation
Backup Software: Veeam Backup for Microsoft 365
Authentication: Microsoft Entra ID (Modern Authentication)
Repository: External datastore attached through Proxmox
```

---

## Objectives

This lab demonstrates how to:

* deploy a dedicated Microsoft 365 backup server
* configure organization-wide backup jobs
* use modern authentication with Entra ID
* externalize repository storage
* validate tenant discovery workflow
* design a rebuild-friendly backup architecture

---

## Backup Server Configuration

Initial deployment sizing:

```
CPU: 8 vCPU
RAM: 12 GB
OS Disk: 80 GB
Repository: External storage attached via Proxmox
```

Steady-state recommended sizing after first full backup:

```
CPU: 4 vCPU
RAM: 8 GB
```

This reflects resource requirements during initial tenant discovery vs incremental backup lifecycle.

---

## Repository Design

Repository storage is externalized from the backup server OS disk.

Benefits:

* improves recoverability
* simplifies server rebuild scenarios
* separates compute and storage layers
* mimics enterprise backup architecture design

Repository type:

```
Local storage presented from Proxmox datastore
```

---

## Microsoft 365 Organization Connection

Connection method:

```
Modern authentication
Certificate-based Entra ID App Registration
```

Steps performed:

1. Create App Registration in Microsoft Entra ID
2. Assign Microsoft Graph permissions
3. Upload authentication certificate
4. Connect organization inside Veeam Backup for Microsoft 365

---

## Backup Job Configuration

Backup scope:

```
Entire organization
```

Protected services:

```
Exchange Online
SharePoint Online
OneDrive for Business
Microsoft Teams
```

Target repository:

```
External datastore repository
```

Initial execution:

```
Manual first run
```

The first execution performs tenant structure discovery before processing backup data.

---

## First Backup Execution Behavior

During the first execution phase:

* mailboxes are discovered
* SharePoint sites are indexed
* OneDrive accounts are enumerated
* Teams structure is processed

Temporary console reconnections may occur during this phase and are expected behavior during initial tenant indexing.

---

## Snapshot Baseline Strategy

After successful deployment, a Proxmox snapshot baseline was created:

```
BASELINE-VEEAM-M365
```

Purpose:

Provide a fast rollback point in case of backup server failure or configuration rollback requirements.

---

## Server Rebuild Scenario

Because repository storage is externalized, the backup server can be redeployed without data loss.

Recovery workflow:

```
Deploy new Windows Server VM
Install Veeam Backup for Microsoft 365
Reconnect existing repository
Reconnect Entra ID App Registration
Resume backup jobs
```

This design improves resilience and rebuild flexibility.

---

## Restore Capabilities

The deployed solution supports restore operations for:

```
Exchange mailboxes
SharePoint documents
OneDrive files
Microsoft Teams data
```

Restore Explorer modules are included with Veeam Backup for Microsoft 365 installation.

---

## Technologies Used

```
Proxmox VE
Windows Server 2022
Veeam Backup for Microsoft 365
Microsoft Entra ID
Microsoft Graph API
Microsoft 365
```

---

## Project Purpose

This repository is part of a broader infrastructure lab focused on:

* Microsoft 365 administration
* endpoint management with Intune
* identity protection with Entra ID
* backup architecture design
* virtualization infrastructure with Proxmox
