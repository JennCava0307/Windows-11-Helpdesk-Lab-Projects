# Windows-11-Helpdesk-Lab-Projects

# Lab 01: Local User Provisioning & NTFS Access Control (Windows 11 Pro)

## 1. Business Scenario & Objective
A small office requires a shared local directory (`C:\Lab_Shared`) on a Windows 11 Pro workstation where authorized team members can create, edit, and delete files, while unauthorized standard users are restricted from accessing or modifying the folder.

**Objective:** Provision role-based local user accounts and security groups, break NTFS permission inheritance, enforce the principle of least privilege, and validate access controls through live user testing and Windows Effective Access.

---

## 2. Lab Environment
* **Hypervisor:** Oracle VirtualBox (macOS Host)
* **Guest OS:** Windows 11 Pro
* **Administrative Tools Used:** Local Users and Groups (`lusrmgr.msc`), File Explorer Security / Advanced NTFS Permissions, Windows Effective Access

---

## 3. Implementation Steps

### A. User & Security Group Provisioning
1. Opened **Local Users and Groups** (`lusrmgr.msc`) to manage workstation accounts locally.
2. Created standard user accounts (`LabUser1` for authorized team testing; `LabUser2` for unauthorized user testing).
3. Created a custom local security group named **`Lab_Team`** and assigned `LabUser1` as a member.

### B. NTFS Folder & Least-Privilege Configuration
1. Created the target directory at `C:\Lab_Shared`.
2. Opened **Properties > Security > Advanced** and disabled permission inheritance (converting inherited permissions into explicit entries) to prevent unintended broad user access.
3. Removed broad standard user permissions while preserving `SYSTEM` and `Administrators`.
4. Added the **`Lab_Team`** security group with explicit **Modify** permissions (allowing Read, Write, Read & execute, and file modification without granting Full Control/ownership changes).

---

## 4. Verification & Testing Results

| Test Case | Account Tested | Group Membership | Expected Result | Actual Result |
| :--- | :--- | :--- | :--- | :--- |
| **Authorized Write Test** | `LabUser1` | `Lab_Team` | Open `C:\Lab_Shared` and create/save `test_note.txt` | **Pass** — File created and saved without error |
| **Unauthorized Access Test** | `LabUser2` | None (Standard User) | Blocked from opening/modifying `C:\Lab_Shared` | **Pass** — Access denied as expected |
| **Permission Audit** | `LabUser2` | None | Confirmed restricted permissions via **Effective Access** tab | **Pass** — Verified least-privilege enforcement |

---

## 5. Key Takeaways & Troubleshooting Notes
* **Group-Based Administration:** Assigning NTFS permissions to `Lab_Team` rather than individual user accounts simplifies future onboarding and offboarding.
* **Least Privilege:** Granting **Modify** instead of **Full Control** allows team members to collaborate on files without altering folder permissions or taking ownership.
* **Diagnostic Tools:** Verified local search scope (**Locations**) when resolving group names and used **Effective Access** to audit permissions cleanly.
