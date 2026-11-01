# Windows Server 2025 File Server Configuration

## Project Overview

Deployed and configured a Windows Server 2025 file server for the 3K Technology Active Directory environment.

The objective was to provide centralized departmental file storage while implementing role-based access control using Active Directory security groups, SMB shares, and NTFS permissions.

This project demonstrates practical experience with Windows Server administration, file services, storage configuration, and access management.

---

## 1. Lab Environment

| Component               | Configuration                   |
| ----------------------- | ------------------------------- |
| Organization            | 3K Technology                   |
| Active Directory Domain | `corp.3ktechnology.test`        |
| Domain Controller       | DC01                            |
| File Server             | FS01                            |
| Operating System        | Windows Server 2025             |
| File Server IP          | `192.168.148.20`                |
| Domain Controller IP    | `192.168.148.10`                |
| Client Workstation      | CLIENT01 — Windows 11           |
| Network                 | VMware NAT — `192.168.148.0/24` |

FS01 was joined to the Active Directory domain to enable centralized authentication and security group-based authorization.

---

## 2. File Server Role Installation

Installed the File Server role service through Windows Server Manager.

The File Server role enables Windows Server to host shared folders accessible through the Server Message Block (SMB) protocol.

### Administrative Tasks

1. Configured Windows Server 2025 on FS01.
2. Joined FS01 to `corp.3ktechnology.test`.
3. Installed the File Server role service.
4. Configured additional storage.
5. Created departmental folders and SMB shares.
6. Applied departmental access permissions.

---

## 3. Storage Configuration

Configured an additional virtual disk to separate company data from the operating system.

| Storage Component     | Configuration |
| --------------------- | ------------- |
| Operating System Disk | 60 GB         |
| Additional Data Disk  | 10 GB         |
| Partition Style       | GPT           |
| File System           | NTFS          |
| Drive Letter          | F:            |
| Volume Label          | CompanyData   |

### Departmental Folder Structure

```text
F:\
└── Shares\
    ├── IT\
    ├── HR\
    ├── Finance\
    └── Sales\
```

Separating company data from the operating system provides a more organized storage structure and simplifies future storage management.

---

## 4. SMB Share Configuration

Created four departmental SMB shares.

| Department      | Network Share    | Local Path          |
| --------------- | ---------------- | ------------------- |
| IT              | `\\FS01\it`      | `F:\Shares\IT`      |
| Human Resources | `\\FS01\hr`      | `F:\Shares\HR`      |
| Finance         | `\\FS01\finance` | `F:\Shares\Finance` |
| Sales           | `\\FS01\sales`   | `F:\Shares\Sales`   |

These shares provide centralized file storage for each department.

### SMB Share Permissions

Configured share permissions using Active Directory security groups.

| Share   | Authorized Group        | Permission |
| ------- | ----------------------- | ---------- |
| IT      | `CORP\SG_IT_Users`      | Change     |
| HR      | `CORP\SG_HR_Users`      | Change     |
| Finance | `CORP\SG_Finance_Users` | Change     |
| Sales   | `CORP\SG_Sales_Users`   | Change     |

Administrative accounts retain the necessary management permissions.

### Screenshot Evidence

Add the screenshot showing the four SMB shares here.

---

## 5. NTFS Permissions

Configured NTFS permissions to restrict access to departmental folders.

Each department was assigned permissions through its corresponding Active Directory security group.

### Departmental Access Control

| Folder  | Security Group     | NTFS Permission |
| ------- | ------------------ | --------------- |
| IT      | `SG_IT_Users`      | Modify          |
| HR      | `SG_HR_Users`      | Modify          |
| Finance | `SG_Finance_Users` | Modify          |
| Sales   | `SG_Sales_Users`   | Modify          |

### Security Configuration

* Disabled inherited permissions on departmental folders.
* Removed unnecessary broad user access.
* Assigned departmental security groups.
* Preserved required administrative and SYSTEM permissions.
* Used group-based access rather than assigning permissions individually.

### Why Both SMB and NTFS Permissions Matter

SMB permissions control access through the network share.

NTFS permissions control access to files and folders on the Windows file system.

When accessing files over SMB, both permission sets apply. The effective access is limited by the combined restrictions.

---

## 6. Active Directory Integration

The file server uses Active Directory security groups to control departmental access.

### Security Groups

```text
corp.3ktechnology.test
│
└── Groups
    ├── SG_IT_Users
    ├── SG_HR_Users
    ├── SG_Finance_Users
    └── SG_Sales_Users
```

### Access Control Design

```text
Active Directory User
        │
        ▼
Department Security Group
        │
        ▼
SMB Share Permissions
        │
        ▼
NTFS Folder Permissions
        │
        ▼
Authorized Department Folder
```

This approach simplifies permission administration.

When employees join or leave a department, access can be managed through security group membership.

---

## 7. File Server Access Testing

Performed access testing from the Windows 11 domain-joined workstation CLIENT01.

### Test Account

**Username:** `CORP\jcarter`

**Department:** IT

**Security Group:** `SG_IT_Users`

### Test Results

| Test                      | Expected Result | Observed Result |
| ------------------------- | --------------- | --------------- |
| Access `\\FS01\it`        | Allowed         | PASS            |
| Create a file in IT share | Allowed         | PASS            |
| Access `\\FS01\hr`        | Denied          | PASS            |
| Access `\\FS01\finance`   | Denied          | PASS            |
| Access `\\FS01\sales`     | Denied          | PASS            |

Successfully created and saved `IT-Access-Test.txt` in the IT share.

Attempts to access other departmental shares returned access-denied results.

### Security Validation

These results demonstrate that the tested IT account could access its authorized departmental share while being restricted from other departmental resources.

Additional testing with accounts from other departments would provide broader validation.

---

## 8. File Server Troubleshooting

During deployment, the File Server role installation encountered the following error:

`0xC004000D`

The installation issue was resolved, allowing the File Server role service to be installed successfully.

The error and resolution process are retained as part of the lab's troubleshooting evidence.

---

## 9. Technical Skills Demonstrated

* Windows Server 2025 administration
* Active Directory domain integration
* File Server role installation
* Virtual disk provisioning
* GPT partitioning
* NTFS volume configuration
* SMB share creation
* NTFS permission management
* Active Directory security groups
* Role-based access control
* File access troubleshooting
* Windows client authentication
* Security testing and validation

---

## 10. Project Outcome

Successfully deployed a centralized Windows Server 2025 file server integrated with Active Directory.

Configured four departmental SMB shares and implemented group-based access controls using SMB and NTFS permissions.

Validated the configuration from a domain-joined Windows 11 workstation by confirming authorized access to the IT share and denied access to other departments.

The completed environment demonstrates foundational skills in enterprise file services, identity management, and Windows Server security administration.

---

## Related Documentation

* [Active Directory Deployment](02-Active-Directory-Setup.md)
* [Group Policy Configuration](03-Group-Policy.md)
* [Main Project README](../README.md)
* [Network Architecture Diagram](../network-architecture.png)
