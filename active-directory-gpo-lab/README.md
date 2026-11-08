# 🏢 Active Directory + Group Policy Security Lab

## 📌 Project Overview

This project demonstrates the deployment and administration of an **Active Directory Domain Services (AD DS)** environment hosted on **Microsoft Azure Virtual Machines**.

The lab focuses on building a Windows domain environment and applying **Group Policy security controls** to centrally manage users and computers.

---

## 🛠️ Technologies Used

- ☁️ Microsoft Azure
- 🖥️ Azure Virtual Machines
- 🪟 Windows Server
- 🏢 Active Directory Domain Services (AD DS)
- 🌐 DNS
- 🔐 Group Policy
- 💻 PowerShell
- 🖥️ Windows Client

---

## 🎯 Lab Objectives

The objective of this lab was to build a functional Active Directory environment in Microsoft Azure and gain hands-on experience with **identity management, domain administration, Group Policy, and Windows security controls**.

The environment consisted of a Windows Server VM configured as a **Domain Controller** and a Windows client VM joined to the Active Directory domain.

---

## ✅ Key Tasks Completed

- ☁️ Deployed Windows Server and Windows client VMs in Microsoft Azure
- 🏢 Installed Active Directory Domain Services
- 👑 Promoted Windows Server to a Domain Controller
- 🌐 Configured DNS for the domain environment
- 🔗 Joined a Windows client VM to the Active Directory domain
- 👥 Created and managed Active Directory users
- 📂 Created and managed Organizational Units (OUs)
- 🔑 Configured domain password policies
- 🔒 Configured account lockout policies
- ⚙️ Created and applied Group Policy Objects (GPOs)
- 🚫 Restricted Control Panel access using Group Policy
- 🛡️ Configured Windows security policies
- 🔄 Tested policy deployment using `gpupdate`
- 📊 Validated applied policies using `gpresult`
- 📄 Generated Group Policy reports for documentation and evidence

---

## 🔐 Security Controls Implemented

The lab demonstrates several important Windows domain security controls, including:

- Strong password requirements
- Account lockout protection
- Centralised security configuration
- User restrictions through Group Policy
- Windows Defender configuration
- Windows Firewall configuration
- Policy enforcement and validation

---

## 🧪 Validation & Testing

After configuring the Group Policy Objects, I tested the policies from the domain-joined Windows client.

Commands such as:

`gpupdate /force`

and:

`gpresult /r`

were used to confirm that the required policies were successfully applied.

---

## 📸 Screenshots

Evidence of the lab configuration, implementation, and testing is available in the [`screenshots`](./screenshots/) folder.

The screenshots document areas such as:

- Domain Controller configuration
- Active Directory Users and Computers
- Organizational Units
- Domain-joined client
- Group Policy Management
- Password policies
- Account lockout policies
- Control Panel restrictions
- Windows security policies
- GPUpdate testing
- GPResult validation

---

## 🧠 Skills Demonstrated

This project demonstrates practical experience with:

- Microsoft Azure administration
- Windows Server administration
- Active Directory administration
- Identity and access management
- DNS configuration
- Group Policy management
- Windows security hardening
- PowerShell
- Troubleshooting
- Technical documentation

---

## 🚀 Project Outcome

Successfully built an **Active Directory Domain Services environment on Microsoft Azure**, connected a Windows client to the domain, implemented centralized security controls using **Group Policy**, and validated policy deployment using Windows administrative tools.
