# Active Directory, Group Policy & Windows Security Hardening Lab

![Windows Server](https://img.shields.io/badge/Windows%20Server-0078D4?style=for-the-badge&logo=windows&logoColor=white)
![Active Directory](https://img.shields.io/badge/Active%20Directory-0078D4?style=for-the-badge&logo=microsoft&logoColor=white)
![Group Policy](https://img.shields.io/badge/Group%20Policy-Security-2E8B57?style=for-the-badge)
![Security](https://img.shields.io/badge/Security-Hardening-C62828?style=for-the-badge)

## Project Overview

This project demonstrates the deployment, administration and security hardening of a **Windows Server Active Directory environment** using **Group Policy Objects (GPOs)**.

The lab applies centralised security controls for authentication, endpoint protection, user environments, removable storage, Windows updates and organisational resources.

### Key Areas

- Password security
- Account lockout protection
- Microsoft Defender
- Windows Defender Firewall
- Departmental file shares
- Corporate desktop standardisation
- Removable storage restrictions
- Windows Update management
- Group Policy Preferences & Drive Mapping

---

## Project Objective

The objective was to simulate how **Active Directory and Group Policy can centrally manage and secure Windows systems in an enterprise environment**.

The project follows a simple security lifecycle:

```text
Active Directory
      ↓
Group Policy
      ↓
Security Controls
      ↓
Endpoint Configuration
      ↓
Policy Enforcement
      ↓
Validation
```

---

## Lab Environment

| Technology | Purpose |
|---|---|
| Windows Server | Server infrastructure |
| Active Directory Domain Services | Identity and domain management |
| DNS | Domain name resolution |
| Group Policy Management | Centralised policy administration |
| Microsoft Defender | Endpoint security |
| Windows Defender Firewall | Host-based network protection |
| Windows File Services | Departmental resource sharing |
| Windows Update | Patch and update management |

---

# Implementation

## 1. Group Policy Management

Group Policy Management was used as the central administration point for configuring domain security and user policies.

```text
Server Manager
      ↓
Tools
      ↓
Group Policy Management
      ↓
Domain
      ↓
Group Policy Objects
```

### Screenshot:

![Group Policy Management](screenshots/01-open-group-policy-management.png)

![Default Domain Policy](screenshots/02-edit-default-domain-policy.png)

> **Outcome:** Established centralised policy administration for domain-managed systems.

---

## 2. Password Security

Domain password policies were configured under:

```text
Computer Configuration
└── Policies
    └── Windows Settings
        └── Security Settings
            └── Account Policies
                └── Password Policy
```

### Implemented Controls

| Security Control | Configuration |
|---|---:|
| Password history | 10 passwords |
| Maximum password age | 90 days |
| Minimum password age | 1 day |
| Minimum password length | 12 characters |
| Password complexity | Enabled |
| Reversible encryption | Disabled |

### Screenshot:

![Password Policy](screenshots/03-password-policy.png)

![Password History](screenshots/04-password-history.png)

![Maximum Password Age](screenshots/05-maximum-password-age.png)

![Minimum Password Age](screenshots/06-minimum-password-age.png)

![Minimum Password Length](screenshots/07-minimum-password-length.png)

> **Security value:** Strengthens authentication by increasing password complexity and reducing password reuse.

---

## 3. Account Lockout Protection

Account lockout controls were implemented to reduce exposure to repeated password-guessing attempts.

### Configuration

```text
Account lockout threshold : 3 invalid attempts
Account lockout duration  : 10 minutes
Reset lockout counter     : 10 minutes
```

### Screenshot:

![Account Lockout Threshold](screenshots/08-account-lockout-threshold.png)

![Suggested Lockout Settings](screenshots/09-account-lockout-suggested-settings.png)

![Final Account Lockout Policy](screenshots/10-final-account-lockout-policy.png)

> **Security value:** Limits repeated authentication attempts and helps reduce basic brute-force password attacks.

---

## 4. Microsoft Defender Security Policies

Microsoft Defender Antivirus policies were managed through Group Policy.

```text
Computer Configuration
└── Administrative Templates
    └── Windows Components
        └── Microsoft Defender Antivirus
            └── Real-time Protection
```

### Security Areas

- Real-time protection
- Behaviour monitoring
- File and program activity monitoring

### Screenshot:

![Defender Real-Time Protection](screenshots/11-windows-defender-real-time-protection.png)

![Defender Behaviour Monitoring](screenshots/12-defender-behavior-monitoring.png)

![Defender File Monitoring](screenshots/13-defender-file-monitoring.png)

> **Security value:** Demonstrates centrally managed endpoint protection and behavioural threat-monitoring capabilities.

---

## 5. Windows Defender Firewall

Windows Defender Firewall with Advanced Security was accessed through Group Policy for centralised host-based network security configuration.

### Screenshot:

![Windows Defender Firewall](screenshots/14-windows-firewall.png)

> **Security value:** Windows Defender Firewall provides endpoint-level traffic filtering and complements network-level security controls.

---

## 6. Departmental File Sharing

Departmental folders were created to simulate shared organisational resources.

```text
C:\DepartmentShares
│
├── Finance
├── HR
└── IT
```

Advanced Sharing was then used to expose the required resources to the network.

### Screenshot:

![Department Shares](screenshots/15-department-shares.png)

![Advanced File Sharing](screenshots/16-advanced-file-sharing.png)

> **Security value:** Demonstrates centralised resource management that can be combined with Active Directory security groups and NTFS permissions for role-based access.

---

## 7. Corporate Desktop Standardisation

Group Policy was used to configure a standard desktop wallpaper for domain users.

```text
User Configuration
└── Policies
    └── Administrative Templates
        └── Desktop
            └── Desktop
                └── Desktop Wallpaper
```

### Configuration

```text
Wallpaper : C:\Corporate\wallpaper.jpg
Style     : Fit
```

### Screenshot:

![Corporate Desktop Wallpaper](screenshots/17-corporate-desktop-wallpaper.png)

> **Operational value:** Demonstrates centralised management and standardisation of the Windows user environment.

---

## 8. Removable Storage Security

A Group Policy control was implemented to prevent users from writing data to removable disks.

```text
User Configuration
└── Policies
    └── Administrative Templates
        └── System
            └── Removable Storage Access
                └── Removable Disks: Deny write access
```

### Configuration

```text
Removable Disks: Deny write access → Enabled
```

### Screenshot:

![Removable Storage Write Protection](screenshots/18-removable-storage-write-protection.png)

> **Security value:** Helps reduce unauthorised data transfer, uncontrolled USB usage and data exfiltration through removable storage.

---

## 9. Windows Update Management

Windows Update policies were centrally configured through Group Policy to improve patch-management control.

### 9.1 Automatic Updates

```text
Configure Automatic Updates        : Enabled
Update Mode                         : Auto download and notify for install
Install during automatic maintenance: Enabled
Scheduled install day              : Every day
Scheduled install time             : 03:00
```

### Screenshot:

![Windows Update Policy](screenshots/19-windows-update-policy.png)

### 9.2 Quality Update Deadline

A deadline was also configured for quality updates.

```text
Policy       : Enabled
Deadline     : 7 days
Grace Period : 2 days
```

### Screenshot:

![Windows Update Deadline](screenshots/20-windows-update-deadline.png)

> **Security value:** Centralised patch management helps reduce the period during which endpoints remain exposed to vulnerabilities addressed by available security updates.

---

## 10. Group Policy Preferences & Drive Mapping

Group Policy Preferences was explored for centrally mapping network resources to domain users.

```text
User Configuration
└── Preferences
    └── Windows Settings
        └── Drive Maps
```

The intended design allows departmental resources to be presented consistently to authorised users.

```text
Finance → F:
HR      → H:
IT      → I:
```

### Screenshot:

![Group Policy Drive Maps](screenshots/21-group-policy-drive-maps.png)

> **Operational value:** Centralised drive mapping reduces manual endpoint configuration and provides users with consistent access to organisational resources.

---

# Security Architecture

The lab combines multiple Windows security layers:

```text
                    DOMAIN ENVIRONMENT
                           │
                           ▼
                 Active Directory (AD DS)
                           │
                           ▼
                    Group Policy
                           │
          ┌────────────────┼────────────────┐
          │                │                │
          ▼                ▼                ▼
     Authentication    Endpoint         User Controls
       Security        Security
          │                │                │
          ├─ Password      ├─ Defender      ├─ Wallpaper
          ├─ Lockout       ├─ Firewall      ├─ Drive Maps
          │                ├─ Updates       └─ USB Controls
          │                │
          ▼                ▼
       Identity         Endpoint
      Protection        Hardening
```

---

# Validation Strategy

Configuration alone does not demonstrate successful policy enforcement.

The lab therefore follows the principle:

```text
Configure
    ↓
Apply
    ↓
Enforce
    ↓
Validate
```

Useful Windows commands for policy validation include:

```powershell
gpupdate /force
```

For verifying policies applied to the machine or user:

```powershell
gpresult /r
```

A detailed HTML Group Policy report can also be generated:

```powershell
gpresult /h GPReport.html
```

The generated report can be stored under:

```text
reports/
└── group-policy-report.html
```

> ⚠️ **Security Note:** Review generated reports before publishing them publicly. Remove or redact internal IP addresses, usernames, hostnames, domain details or other environment-specific information that should not be exposed.

---

# Controls Implemented

| Area | Control | Status |
|---|---|---|
| Authentication | Password policy | ✅ Configured |
| Authentication | Account lockout | ✅ Configured |
| Endpoint | Defender policies | ✅ Configured / explored |
| Network | Windows Firewall | ✅ Policy area configured/explored |
| Resources | Department shares | ✅ Created |
| User Environment | Corporate wallpaper | ✅ Configured |
| Data Protection | Removable disk write restriction | ✅ Enabled |
| Patch Management | Automatic updates | ✅ Enabled |
| Patch Management | Quality update deadline | ✅ Enabled |
| Resources | Drive mapping | ✅ Configuration stage |

---

# Screenshots For Validation Evidence:

The following evidence can be added as the lab develops:

- [ ] Domain-joined client machine
- [ ] `gpupdate /force` execution
- [ ] `gpresult /r` results
- [ ] GPO HTML report
- [ ] Corporate wallpaper applied to client
- [ ] Departmental drives mapped to client
- [ ] NTFS permissions for Finance, HR and IT
- [ ] Active Directory security groups
- [ ] Removable storage restriction tested
- [ ] Account lockout tested
- [ ] Final Microsoft Defender configuration
- [ ] Final Windows Firewall configuration

---

# Skills Demonstrated

### Microsoft Infrastructure
- Windows Server Administration
- Active Directory Domain Services
- DNS
- Group Policy Management
- Windows File Services

### Identity & Security
- Password policy administration
- Account lockout controls
- Group Policy security hardening
- Microsoft Defender configuration
- Windows Defender Firewall
- Removable storage restrictions

### Endpoint Management
- Centralised Windows configuration
- Windows Update management
- Desktop standardisation
- Group Policy Preferences
- Network drive mapping

### Security Operations
- Endpoint hardening
- Access-control concepts
- Patch management
- Policy validation
- Security evidence collection

---

# Key Takeaway

This project demonstrates more than deploying an Active Directory domain.

It demonstrates how **Active Directory, Group Policy and native Windows security controls work together to centrally manage, standardise and harden an enterprise Windows environment.**

The project follows an administration model of:

> **Identity → Policy → Security → Enforcement → Validation
