# Architecture Overview – Microsoft 365 Backup Lab

## Logical Backup Flow

```
Microsoft 365 Tenant
        │
        ▼
Veeam Backup Server (Windows Server VM)
        │
        ▼
External Backup Repository (Proxmox Datastore)
        │
        ▼
Restore Explorers (Exchange / SharePoint / OneDrive / Teams)
```

## Infrastructure Components

The backup environment is deployed on a virtualized infrastructure using:

* Proxmox VE hypervisor
* Dedicated Windows Server backup VM
* External repository storage
* Microsoft Entra ID modern authentication

Logical architecture:

Microsoft 365 Tenant
↓
Veeam Backup for Microsoft 365 Server (VM)
↓
External Repository Storage (Proxmox datastore)

---

## Design Decisions

### Dedicated Backup Server

Backup services run on a dedicated VM to isolate backup workloads from other infrastructure components.

Benefits:

* improved stability
* simplified troubleshooting
* rebuild-friendly architecture

---

### External Repository Storage

Repository storage is separated from the operating system disk.

Benefits:

* protects backup data from OS failure
* enables fast server rebuild scenarios
* mimics enterprise backup architecture

---

### Organization-Wide Backup Scope

Backup scope includes:

* Exchange Online
* SharePoint Online
* OneDrive for Business
* Microsoft Teams

This ensures full tenant coverage.

---

## Snapshot Strategy

A Proxmox snapshot baseline was created after deployment:

BASELINE-VEEAM-M365

Purpose:

Provide rollback capability after configuration changes.
