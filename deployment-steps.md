# Deployment Steps – Veeam Backup for Microsoft 365

## Server Deployment

Create a dedicated Windows Server VM on Proxmox:

Recommended configuration:

```
CPU: 8 vCPU
RAM: 12 GB
Disk: 80 GB OS disk
Repository: External storage attached via Proxmox datastore
```

Install Windows Server 2022 Evaluation.

Apply Windows Updates before installing Veeam Backup for Microsoft 365.

---

## Install Veeam Backup for Microsoft 365

Mount the Veeam installation ISO.

Run:

```
setup.exe
```

Install:

* Backup Server
* Backup Console
* Explorer for Exchange
* Explorer for SharePoint
* Explorer for OneDrive
* Explorer for Teams

Console access:

```
localhost:9191
```

---

## Configure Backup Repository

Navigate to:

```
Backup Infrastructure → Backup Repositories
```

Create repository:

```
Type: Local storage
Location: External datastore mounted via Proxmox
```

Purpose:

Separate compute layer from storage layer.

---

## Connect Microsoft 365 Organization

Navigate to:

```
Organizations → Add Organization
```

Select:

```
Modern authentication
```

Use:

```
Certificate-based authentication
```

Steps:

1. Create Entra ID App Registration
2. Assign Microsoft Graph permissions
3. Upload certificate
4. Connect tenant inside Veeam console

---

## Create Backup Job

Navigate to:

```
Backup Jobs → New Backup Job
```

Select:

```
Entire organization
```

Workloads included:

```
Exchange Online
SharePoint Online
OneDrive for Business
Microsoft Teams
```

Repository:

```
External datastore repository
```

Run initial backup manually.

