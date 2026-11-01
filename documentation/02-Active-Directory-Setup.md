# Active Directory Deployment and Configuration

## Overview

Deployed Active Directory Domain Services (AD DS) on Windows Server 2025 to provide centralized authentication and identity management for a simulated organization, 3K Technology.

## Domain Controller Configuration

| Setting          | Value                  |
| ---------------- | ---------------------- |
| Server Name      | DC01                   |
| Operating System | Windows Server 2025    |
| Domain           | corp.3ktechnology.test |
| IP Address       | 192.168.148.10         |
| Subnet Mask      | 255.255.255.0          |
| Default Gateway  | 192.168.148.2          |
| DNS Server       | DC01                   |

DC01 was promoted to the first domain controller in a new Active Directory forest.

The server provides Active Directory Domain Services and DNS for the lab environment.

## Organizational Unit Structure

![Active Directory OU Structure](../screenshots/active-directory/01-ou-structure.png)

Created the following organizational structure to separate administrative objects by function.

```text
corp.3ktechnology.test
└── 3K-Technology
    ├── Computers
    ├── Disabled-Accounts
    ├── Groups
    ├── Servers
    └── Users
        ├── Finance
        ├── HR
        ├── IT
        └── Sales
```

This structure supports departmental organization, delegated administration, and targeted Group Policy deployment.

## Employee Account Management

Created eight employee accounts across four departments.

| Department | Employee       | Username  |
| ---------- | -------------- | --------- |
| IT         | James Carter   | jcarter   |
| IT         | Marcus Johnson | mjohnson  |
| HR         | Sarah Williams | swilliams |
| HR         | Ashley Brown   | abrown    |
| Finance    | Michael Davis  | mdavis    |
| Finance    | Jessica Taylor | jtaylor   |
| Sales      | David Wilson   | dwilson   |
| Sales      | Emily Anderson | eanderson |

## Security Groups

![Department Security Groups](../screenshots/active-directory/07-security-groups.png)

Created four Global Security groups:

| Group            | Purpose                   |
| ---------------- | ------------------------- |
| SG_IT_Users      | IT department access      |
| SG_HR_Users      | Human Resources access    |
| SG_Finance_Users | Finance department access |
| SG_Sales_Users   | Sales department access   |

These groups are used to assign permissions to departmental file shares hosted on FS01.

Group-based access management reduces the need to assign permissions directly to individual users.

## Domain-Joined Workstation

![Domain Authentication Verification](../screenshots/active-directory/09-domain-login.png)

Deployed a Windows 11 virtual machine named CLIENT01.

Configured CLIENT01 to use DC01 for DNS resolution and joined the workstation to the domain.

Validated domain authentication by signing in as `CORP\jcarter`.

Verification commands:

```cmd
whoami
echo %LOGONSERVER%
```

Observed results:

```text
corp\jcarter
\\DC01
```

These results confirmed that CLIENT01 successfully authenticated using a domain account.

## DNS Validation

Validated DNS functionality using PowerShell and Windows Server diagnostic tools.

```powershell
Resolve-DnsName corp.3ktechnology.test -Server 192.168.148.10
dcdiag /test:dns /v
```

The domain resolved to the domain controller's IP address, and DNS diagnostic tests passed.

## Outcome

Successfully established a centralized Active Directory environment supporting domain authentication, departmental account organization, security groups, and domain-joined Windows clients.
