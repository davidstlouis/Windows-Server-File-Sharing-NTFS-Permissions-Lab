# Windows Server File Sharing & NTFS Permissions Lab

## Project Overview

This project demonstrates how to configure and manage a Windows file server using **Windows Server 2022** and **Windows 10 Enterprise** virtual machines hosted in Microsoft Azure.

The objective was to create departmental shared folders, configure NTFS and share permissions, manage access through Active Directory security groups, and troubleshoot file access issues.

## Technologies Used

- Microsoft Azure
- Windows Server 2022 Datacenter: Azure Edition
- Windows 10 Enterprise
- Active Directory Domain Services (AD DS)
- NTFS Permissions
- SMB File Sharing
- Command Prompt

---

## Lab Environment

```text
Microsoft Azure
│
├── Windows Server 2022 (SERVER01)
│   ├── Active Directory
│   ├── DNS Server
│   └── File Server
│       └── CompanyShares
│           ├── IT
│           └── HR
│
└── Windows 10 Enterprise (CLIENT01)
    └── Joined to davidlab.local
```

## Step 1: Configure Active Directory Security Groups

Using Active Directory Users and Computers, I created two Global Security Groups:

- `IT-File-Access`
- `HR-File-Access`

I assigned the appropriate domain users to each group to manage departmental access.

<img width="1470" height="956" alt="Screenshot 2026-10-07 at 7 59 58 PM" src="https://github.com/user-attachments/assets/840bf4e8-a786-4ae3-a09d-7503d1e8c995" />
<img width="1470" height="956" alt="Screenshot 2026-10-07 at 8 00 16 PM" src="https://github.com/user-attachments/assets/481eb6f8-e979-4d2f-a98c-38711965b9a3" />

---

## Step 2: Create Departmental Shared Folders

On Windows Server 2022, I created a directory structure for departmental file sharing.

```text
C:\CompanyShares
├── IT
└── HR
```

These folders serve as shared storage locations for authorized employees.


<img width="1470" height="956" alt="Screenshot 2026-10-07 at 8 01 52 PM" src="https://github.com/user-attachments/assets/90c412dc-48be-485e-b4ac-ce97ebc02a8f" />
---

## Step 3: Configure NTFS Permissions

I configured NTFS permissions to restrict folder access based on Active Directory security group membership.

| Folder | Security Group | Permission |
|---|---|---|
| IT | IT-File-Access | Modify |
| HR | HR-File-Access | Modify |
| Both | Administrators / SYSTEM | Full Control |

This configuration allows authorized employees to access and modify their department's files while restricting unauthorized access.


<img width="1470" height="956" alt="Screenshot 2026-10-07 at 8 18 00 PM" src="https://github.com/user-attachments/assets/f5f236d9-2800-4500-a6e5-e54d571f612e" />
<img width="1470" height="956" alt="Screenshot 2026-10-07 at 8 18 28 PM" src="https://github.com/user-attachments/assets/c02498a0-0a1c-4d92-8de7-411bee97de36" />

---

## Step 4: Configure Network File Sharing

I enabled SMB file sharing and configured share permissions for both departmental folders.

Network share paths:

```text
\\SERVER01\IT
\\SERVER01\HR
```

Each share was configured to allow access to its corresponding Active Directory security group.

**Screenshot:**

<img width="1470" height="956" alt="Screenshot 2026-10-07 at 8 22 10 PM" src="https://github.com/user-attachments/assets/33789d6b-ff62-4200-b238-429aa8602f81" />
<img width="1470" height="956" alt="Screenshot 2026-10-07 at 8 22 53 PM" src="https://github.com/user-attachments/assets/ef2df919-24e7-4cb6-b7b0-958466d8231f" />

---

## Step 5: Test User Access

Using the Windows 10 Enterprise domain-joined client, I tested access with two domain accounts.

**IT User — jsmith**

- Successfully accessed `\\SERVER01\IT`
- Created `IT-Test.txt`
- Verified that access to the HR folder was restricted

**HR User — sjohnson**

- Successfully accessed `\\SERVER01\HR`
- Created `HR-Test.txt`
- Verified that access to the IT folder was restricted

These tests demonstrated that folder permissions were enforced according to group membership.

**Screenshot:**

<!-- Insert successful access and Access Denied screenshots here -->

---

## Step 6: Map a Network Drive

I mapped the authorized departmental share to a network drive on Windows 10.

Example command:

```cmd
net use Z: \\SERVER01\HR /persistent:yes
```

I verified the mapped drive using:

```cmd
net use
```

This demonstrated how employees can access shared network resources through mapped drives.

**Screenshot:**

<!-- Insert mapped network drive screenshot here -->

---

## Step 7: Troubleshoot File Access Permissions

To simulate a common IT support issue, I temporarily removed a user from their departmental security group.

I used the following command to investigate the user's group membership:

```cmd
whoami /groups
```

After identifying the missing group membership, I restored the user to the correct security group and refreshed the user's logon session.

I then verified that access to the shared folder was restored.

**Troubleshooting Summary**

- **Issue:** User could not access their departmental shared folder.
- **Root Cause:** User was missing the required Active Directory security group membership.
- **Resolution:** Restored the correct group membership and refreshed the user's logon session.
- **Verification:** Confirmed successful access to the shared folder.

**Screenshot:**

<!-- Insert troubleshooting screenshot here -->

---

## Skills Demonstrated

- Windows Server Administration
- Active Directory Security Groups
- NTFS Permissions
- SMB File Sharing
- Role-Based Access Control (RBAC)
- Network Drive Mapping
- Identity and Access Management
- Windows Client Administration
- File Access Troubleshooting
- Microsoft Azure

---
