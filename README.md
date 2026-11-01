# 3K Technology | Active Directory & File Server Lab

### Windows Server 2025 • Active Directory • Group Policy • DNS • SMB • NTFS • VMware

## Project Overview

Designed and deployed a virtual enterprise IT environment for a fictional organization, **3K Technology**, to gain hands-on experience administering Windows infrastructure.

The environment simulates a small business with four departments: IT, Human Resources, Finance, and Sales.

The project focuses on centralized identity management, domain authentication, security policies, network file sharing, and role-based access control.

## Lab Architecture

![3K Technology Network Architecture](network-architecture.png)

| System   | Operating System    | Role                      | IP Address     |
| -------- | ------------------- | ------------------------- | -------------- |
| DC01     | Windows Server 2025 | Domain Controller, DNS    | 192.168.148.10 |
| FS01     | Windows Server 2025 | File Server               | 192.168.148.20 |
| CLIENT01 | Windows 11          | Domain-Joined Workstation | DHCP           |

## Project Documentation

The following documentation provides a detailed breakdown of the infrastructure deployment, security configurations, testing procedures, and troubleshooting performed throughout this project.

| Documentation                                                                | Description                                                                                                   |
| ---------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------- |
| [Active Directory Configuration](documentation/02-Active-Directory-Setup.md) | Domain controller deployment, organizational units, user accounts, security groups, and domain authentication |
| [Group Policy Configuration](documentation/03-Group-Policy.md)               | Workstation security baseline, password policies, account lockout settings, and PowerShell verification       |
| [File Server Configuration](documentation/04-File-Server-Configuration.md)   | Windows Server file services, SMB shares, NTFS permissions, and departmental access controls                  |
| [Security Testing](documentation/05-Security-Testing.md)                     | Authorized and unauthorized file access testing using domain accounts                                         |
| [Troubleshooting](documentation/06-Troubleshooting.md)                       | Windows Server installation errors, issue resolution, and post-resolution verification                        |

### Key Project Accomplishments

* Deployed three virtual machines using VMware Workstation.
* Configured Windows Server 2025 as an Active Directory domain controller.
* Created eight employee accounts across four departments.
* Implemented departmental security groups for role-based access control.
* Configured workstation security and domain account policies using Group Policy.
* Deployed a dedicated Windows Server file server with four SMB shares.
* Implemented departmental NTFS and SMB permissions.
* Validated authorized and unauthorized access from a Windows 11 domain-joined workstation.
* Documented deployment challenges and troubleshooting outcomes.


**Active Directory Domain:** `corp.3ktechnology.test`

**Virtualization Platform:** VMware Workstation

**Network Configuration:** VMware NAT

## Active Directory Configuration

Installed and configured Active Directory Domain Services (AD DS) and DNS on DC01.

Created a structured organizational hierarchy to manage departmental users, computers, servers, and security groups.

### Organizational Units

* IT
* HR
* Finance
* Sales
* Computers
* Servers
* Groups
* Disabled Accounts

### User and Group Management

Created eight employee accounts across four departments.

Configured security groups to manage access to departmental resources:

| Department | Security Group   |
| ---------- | ---------------- |
| IT         | SG_IT_Users      |
| HR         | SG_HR_Users      |
| Finance    | SG_Finance_Users |
| Sales      | SG_Sales_Users   |

Used group-based permissions to simplify access management.

## Group Policy Configuration

Configured Group Policy Objects (GPOs) to implement security settings across the domain.

Policies included:

* Workstation inactivity lock
* Account lockout threshold
* Account lockout duration
* Password complexity requirements
* Password length requirements

Used Group Policy Management and PowerShell to inspect and validate policy configurations.

## File Server Deployment

Deployed FS01 as a dedicated Windows Server 2025 file server.

Added a separate 10GB virtual disk, initialized it using GPT, and formatted it with NTFS.

Created departmental folders on the F: drive:

```text
F:\Shares
├── IT
├── HR
├── Finance
└── Sales
```

Configured SMB network shares for each department.

| Department | Network Share    |
| ---------- | ---------------- |
| IT         | `\\FS01\it`      |
| HR         | `\\FS01\hr`      |
| Finance    | `\\FS01\finance` |
| Sales      | `\\FS01\sales`   |

### File Access Security

Implemented departmental access controls using:

* Active Directory security groups
* NTFS permissions
* SMB share permissions

Configured departmental groups with Modify permissions on their respective folders and Change permissions on their network shares.

Restricted unauthorized access to other departments.

## Security Testing

Performed access control testing from CLIENT01 using the domain account `CORP\jcarter`.

| Test                    | Expected Result | Actual Result |
| ----------------------- | --------------- | ------------- |
| Access IT share         | Allowed         | PASS          |
| Create and save IT file | Allowed         | PASS          |
| Access HR share         | Denied          | PASS          |
| Access Finance share    | Denied          | PASS          |
| Access Sales share      | Denied          | PASS          |

The tests verified that the IT employee could access authorized resources while access to other departments was denied.

## Troubleshooting

Encountered and investigated several issues during deployment, including:

* DNS resolution and connectivity issues
* Domain authentication problems
* File Server role installation failure
* Security group lookup and permission con
