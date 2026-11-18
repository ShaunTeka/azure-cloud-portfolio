# Azure Managed Disk Lifecycle Lab

![Microsoft Azure](https://img.shields.io/badge/Microsoft_Azure-0078D4?style=for-the-badge&logo=microsoftazure&logoColor=white)
![Windows Server](https://img.shields.io/badge/Windows_Server-0078D6?style=for-the-badge&logo=windows&logoColor=white)
![Storage](https://img.shields.io/badge/Storage-Premium_SSD-00A4EF?style=for-the-badge)

## Project Overview

This lab demonstrates the **complete lifecycle of an Azure Managed Disk** attached to a Windows Server virtual machine.

The project covers:

- Creating and attaching an Azure Premium SSD.
- Initializing the disk in Windows Server.
- Creating and formatting an NTFS volume.
- Expanding storage from **4 GiB to 8 GiB**.
- Extending the Windows volume to consume the additional capacity.
- Verifying the expanded storage.
- Detaching and identifying the managed disk for reuse.
- Documenting the final resource cleanup process.

The lab demonstrates the relationship between **Azure infrastructure-level storage** and **Windows operating-system-level storage management**.

---

# Objectives

The objectives of this project were to:

- Deploy additional storage to an Azure VM.
- Understand Azure Managed Disk configuration.
- Configure storage inside Windows Server.
- Work with GPT partitions and NTFS volumes.
- Resize Azure storage without recreating the VM.
- Extend a Windows volume after cloud-side resizing.
- Understand the difference between detaching and deleting storage.
- Practice responsible Azure resource cleanup.

---

# Lab Environment

| Component | Configuration |
|---|---|
| Cloud Platform | Microsoft Azure |
| Operating System | Windows Server |
| Azure Region | South Africa North |
| Storage | Azure Managed Disk |
| Storage Type | Premium SSD LRS |
| Initial Data Disk Size | 4 GiB |
| Expanded Data Disk Size | 8 GiB |
| Initial Tier | P1 |
| Expanded Tier | P2 |
| Provisioned IOPS | 120 |
| Provisioned Throughput | 25 MB/s |
| Partition Style | GPT |
| File System | NTFS |
| Drive Letter | D: |
| LUN | 0 |

> 🔐 Sensitive account, subscription, IP address and tenant information are redacted before screenshots were published.

---

# Storage Lifecycle

```text
Windows Server VM
        │
        ▼
Create 4 GiB Premium SSD
        │
        ▼
Attach Disk to VM
        │
        ▼
Assign LUN 0
        │
        ▼
Windows Detects New Disk
        │
        ▼
Initialize Disk using GPT
        │
        ▼
Create NTFS Volume
        │
        ▼
Assign Drive D:
        │
        ▼
Verify Storage
        │
        ▼
Resize Azure Disk
    4 GiB → 8 GiB
        │
        ▼
Windows Detects
Unallocated Capacity
        │
        ▼
Extend D: Volume
        │
        ▼
Verify 8 GiB Storage
        │
        ▼
Detach Managed Disk
        │
        ▼
Verify Unattached Disk
        │
        ▼
Delete Resource
When No Longer Required
```

---

# Implementation

## 1. Review Existing VM Storage

The lab began by reviewing the virtual machine's existing storage configuration.

At this stage, the VM contained its operating-system disk but did not have a separate managed data disk attached.

![Existing OS Disk](screenshots/01-existing-os-disk.png)

This established the baseline configuration before additional storage was provisioned.

---

## 2. Create a Managed Data Disk

A new managed disk was created directly from the VM's **Disks** configuration.

The initial configuration used:

- **Size:** 4 GiB
- **Storage:** Premium SSD LRS
- **LUN:** 0
- **Host caching:** Read-only
- **Encryption:** Server-side encryption with platform-managed keys

![Data Disk Configuration](screenshots/02-data-disk-configuration.png)

The new disk was assigned to **LUN 0**.

![LUN Configuration](screenshots/03-lun-configuration.png)

Azure successfully created the managed disk and updated the VM configuration.

![Disk Creation Successful](screenshots/04-disk-created-successfully.png)

The VM storage configuration now showed the new data disk alongside the operating-system disk.

![Attached Data Disk](screenshots/05-attached-data-disk.png)

---

# Windows Server Storage Configuration

## 3. Verify the New Disk

After connecting to Windows Server, File Explorer initially displayed only the existing **C:** drive.

![Before Initialization](screenshots/06-before-disk-initialization.png)

Attaching a disk in Azure makes the storage available to the VM, but the operating system must still prepare the disk before applications can use it.

---

## 4. Initialize the Disk

Windows Disk Management detected the additional disk as:

- Unknown
- Not initialized
- Approximately 4 GB
- Unallocated

The disk was initialized using **GPT (GUID Partition Table)**.

![Initialize GPT Disk](screenshots/07-initialize-disk-gpt.png)

GPT provides a modern partitioning structure suitable for current Windows Server environments.

---

## 5. Create the Data Volume

Once initialized, the available capacity appeared as unallocated space.

A **New Simple Volume** was created.

![Create Simple Volume](screenshots/08-create-simple-volume.png)

The storage was configured as:

- **File system:** NTFS
- **Drive letter:** D:
- **Partition:** Basic data partition

Disk Management confirmed that the new volume was healthy.

![Completed NTFS Volume](screenshots/09-completed-ntfs-volume.png)

---

## 6. Verify the D: Drive

File Explorer now displayed both:

- Windows **C:**
- New Volume **D:**

![New D Drive](screenshots/10-new-d-drive.png)

The Azure managed-disk overview also confirmed that the data disk had been provisioned successfully.

![Managed Disk Overview](screenshots/11-managed-disk-overview.png)

At this stage, the data disk was operational and available to Windows Server.

---

# Storage Expansion

## 7. Review Disk Performance

The Azure **Size + performance** configuration was reviewed before resizing the disk.

![Disk Size Options](screenshots/12-disk-size-options.png)

The original managed disk used the **4 GiB P1** configuration.

---

## 8. Resize the Azure Managed Disk

The disk was increased from:

**4 GiB → 8 GiB**

![Resize Disk](screenshots/13-resize-disk-to-8-gib.png)

The resulting configuration moved the disk to the **P2** tier while retaining:

- **120 provisioned IOPS**
- **25 MB/s provisioned throughput**

Azure successfully updated the disk.

![Disk Resize Successful](screenshots/14-disk-resize-successful.png)

---

# Extend the Windows Volume

## 9. Detect the Additional Capacity

After the Azure disk was expanded, Windows Disk Management detected the disk as approximately **7.98 GB**.

However, the existing D: partition remained approximately **3.98 GB**.

The additional capacity appeared as approximately **4 GB of unallocated space**.

![Unallocated Capacity](screenshots/15-expanded-disk-unallocated-space.png)

This demonstrates an important cloud administration concept:

> Expanding an Azure Managed Disk does not automatically extend the Windows file-system volume.

The infrastructure and operating-system layers must be managed separately.

---

## 10. Extend the D: Volume

The Windows **Extend Volume Wizard** was used to add the newly available storage to the existing D: partition.

![Extend Volume Wizard](screenshots/16-extend-volume-wizard.png)

After completing the wizard, Disk Management showed one continuous **7.98 GB NTFS D:** volume.

![Expanded Volume](screenshots/17-expanded-volume-disk-management.png)

The unallocated storage had been incorporated successfully.

---

## 11. Final Windows Verification

File Explorer confirmed the expanded drive:

**D: ≈ 7.98 GB**

![Final Drive Verification](screenshots/18-final-drive-verification.png)

This verified that the cloud-side storage expansion and operating-system-side expansion were both successful.

---

# Azure Verification

## 12. Confirm the Final Azure Configuration

The VM's Azure storage configuration confirmed that the attached data disk was now provisioned at:

- **8 GiB**
- Premium SSD LRS
- LUN 0
- 120 IOPS
- 25 MB/s throughput
- Server-side encryption with platform-managed keys

![Azure 8 GiB Disk](screenshots/19-azure-eight-gib-disk-verification.png)

At this stage:

```text
Azure Managed Disk
        │
        │ 8 GiB
        ▼
Windows Server
        │
        ▼
NTFS D: Volume
        │
        ▼
≈ 7.98 GB usable volume
```

The complete storage expansion was therefore validated from both Azure and Windows Server.

---

# Disk Detachment and Reuse

## 13. Detach the Managed Disk

After successful testing, the managed disk was detached from the virtual machine.

![Data Disk Detached](screenshots/20-data-disk-detached.png)

Azure confirmed that the VM configuration had been successfully updated.

![VM Updated](screenshots/21-disk-detachment-successful.png)

### Important distinction

**Detach ≠ Delete**

Detaching removes the relationship between the disk and VM.

The managed disk continues to exist independently in Azure until it is explicitly deleted.

---

## 14. Verify Disk Reusability

The detached disk remained available under **Attach existing disks**.

![Existing Disk Available](screenshots/22-reattach-existing-disk.png)

This demonstrates that Azure managed disks can exist independently of a VM.

A detached disk could potentially be reused for:

- Data recovery.
- VM migration.
- Troubleshooting.
- Rebuilding infrastructure.
- Preserving application data.
- Reattaching storage to another compatible VM.

---

# Managed Disk Inventory

## 15. Review Azure Disk Resources

The Azure disk inventory displayed both the VM operating-system disk and the standalone managed data disk.

![Managed Disk Inventory](screenshots/23-azure-managed-disk-inventory.png)

This provides visibility into storage resources independently of their associated virtual machines.

---

## 16. Confirm the Unattached State

The managed-disk resource showed:

- **Disk state:** Unattached
- **Size:** 8 GiB
- **Storage:** Premium SSD LRS
- **Performance tier:** P2
- **IOPS:** 120
- **Throughput:** 25 MB/s
- **Maximum shares:** 1

![Unattached Disk](screenshots/24-unattached-disk-verification.png)

This confirmed that the disk still existed as an Azure resource even though it was no longer connected to the VM.

---

#  Resource Cleanup

## 17. Remove the Managed Disk

Once the lab was completed and the disk was no longer required, the managed-disk deletion workflow was initiated.

![Delete Disk](screenshots/25-delete-disk-confirmation.png)

Before permanently deleting cloud storage, an administrator should verify that:

- The disk is detached.
- The stored data is no longer required.
- Required data has been backed up.
- A snapshot exists if recovery could be necessary.
- The correct Azure resource has been selected.

> ⚠️ The screenshot documents the deletion confirmation stage. It should not be treated as evidence that deletion completed unless a subsequent Azure confirmation is captured.

Cleaning up unused resources is an important part of cloud administration because detached disks can continue generating Azure storage costs while they exist.

---

# Project Outcome

This project successfully demonstrated the lifecycle of an Azure managed data disk from deployment through cleanup.

### Accomplished

- [x] Reviewed existing VM storage.
- [x] Created a Premium SSD managed disk.
- [x] Attached the disk to an Azure VM.
- [x] Configured LUN 0.
- [x] Initialized the disk using GPT.
- [x] Created an NTFS volume.
- [x] Assigned the D: drive.
- [x] Verified storage in Windows Server.
- [x] Expanded the Azure disk from 4 GiB to 8 GiB.
- [x] Identified unallocated capacity in Windows.
- [x] Extended the existing Windows volume.
- [x] Verified the final 7.98 GB D: volume.
- [x] Detached the managed disk.
- [x] Verified that the disk remained reusable.
- [x] Confirmed the disk's unattached state.
- [x] Documented the Azure cleanup process.

---

# Key Learnings

## 1. Cloud Storage Has Multiple Management Layers

Azure controls the underlying managed disk while Windows Server manages partitions, drive letters and file systems.

```text
Azure
└── Managed Disk: 8 GiB
        │
        ▼
Windows Server
└── Disk
    └── NTFS Volume
        └── D:
```

Changing one layer does not necessarily change the other automatically.

---

## 2. Disk Expansion Is a Two-Step Process

Increasing storage required two operations:

### Azure layer

```text
4 GiB → 8 GiB
```

### Windows layer

```text
3.98 GB D:
+
4 GB Unallocated
↓
7.98 GB D:
```

This was one of the most important findings from the lab.

---

## 3. Detachinghing screenshots to a public GitHub repository:

### Redact

- ❌ Public IP addresses
- ❌ Private IP addresses where unnecessary
- ❌ Subscription IDs
- ❌ Personal or university email addresses
- ❌ Tenant identifiers
- ❌ Credentials
- ❌ Access keys
- ❌ Connection strings
- ❌ Secrets and tokens

### Safe technical evidence to retain

- [x] Disk size
- [x] Storage tier
- [x] LUN
- [x] IOPS
- [x] Throughput
- [x] Partition type
- [x] File system
- [x] Drive letter
- [x] General architecture
- [x] Successful configuration states

> **I did not commit passwords, credentials, Azure access keys, secrets or authentication tokens to GitHub.**


---

# Technologies & Skills Demonstrated

### Microsoft Azure
- Azure Virtual Machines
- Azure Managed Disks
- Premium SSD LRS
- Azure resource management
- Disk performance configuration
- Resource lifecycle management

### Windows Server
- Disk Management
- GPT partitioning
- NTFS
- Volume creation
- Volume extension
- Drive management
- Remote administration

### Cloud Administration
- Infrastructure provisioning
- Storage scaling
- Resource verification
- Infrastructure troubleshooting
- Capacity management
- Resource cleanup
- Cost-awareness

---

# What This Project Demonstrates

This lab goes beyond simply creating an Azure VM.

It demonstrates practical understanding of how cloud infrastructure and the guest operating system interact when managing storage.

The project covers the complete administrative workflow:

**Provision → Attach → Initialize → Format → Verify → Scale → Extend → Validate → Detach → Clean Up**

---

> Part of my hands-on Azure cloud and infrastructure portfolio.
