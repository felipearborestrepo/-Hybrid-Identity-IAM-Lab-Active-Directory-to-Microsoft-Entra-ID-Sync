# 🔐 Hybrid Identity IAM Lab — Active Directory to Microsoft Entra ID Sync

<img width="1536" height="1024" alt="1f147ca4-066c-42c6-bf2b-2814b5078ab4" src="https://github.com/user-attachments/assets/7ef63e0f-33c6-46c9-96fd-f823cd465f45" />

**Author:** Felipe Restrepo  
**Category:** Identity & Access Management / Cloud Security  
**Technologies:** Windows Server, Active Directory, Microsoft Entra ID, Entra Connect, Dynamic Groups, Enterprise Applications, Audit Logs  

---

# 📘 Project Overview

This project demonstrates the implementation of a **hybrid identity architecture** by synchronizing an on-premises **Active Directory Domain Services (AD DS)** environment with **Microsoft Entra ID** using **Microsoft Entra Connect**.

The lab simulates a real enterprise IAM environment where:

- Active Directory is the **source of authority**
- Identities are synchronized to the cloud
- Access is automated using **attribute-based dynamic groups**
- Joiner–Mover–Leaver (JML) lifecycle events are validated
- Audit logs and sign-in logs provide compliance evidence

---

# ✅ What This Project Builds

- Windows Server Domain Controller (Active Directory)  
- Microsoft Entra Connect installed on the DC  
- Password Hash Synchronization enabled  
- Department-based OUs and users  
- Cloud identity synchronization  
- Dynamic group automation  
- Joiner, Mover, Leaver lifecycle validation  
- Audit and sign-in log evidence  

---

# 🧾 Requirements

- Microsoft Entra tenant  
- Windows Server 2019 or 2022 VM  
- Admin rights on the server  
- Optional but recommended: Microsoft Entra ID P1/P2  

---

# 🏗 Architecture Summary

On-prem Active Directory → Microsoft Entra Connect → Microsoft Entra ID → Dynamic Groups → Automated Access

Active Directory remains the authoritative identity source while Entra ID provides cloud access control and auditing.

---

# ⚙️ Implementation Walkthrough

---

## 🔹 PART 0 — VM Deployment & Preparation

### Create Windows Server VM
Azure Portal → Virtual Machines → Create → Azure virtual machine  
- Image: Windows Server 2022 Datacenter  
- Size: 2 vCPU recommended  
- Networking: Allow RDP (3389)  

---

### RDP into the server
VM → Connect → RDP → Download → Sign in

---

## 🔹 PART 1 — Promote Server to Domain Controller

### Install AD DS Role
Server Manager → Manage → Add Roles and Features  
Select: Active Directory Domain Services → Install

📸 Screenshot: AD DS role installed

<img width="1366" height="728" alt="1" src="https://github.com/user-attachments/assets/c45bc57b-8ba8-45c1-9b99-cabc9b19cabb" />

---

### Promote to Domain Controller
Server Manager → ⚠️ → Promote this server  
- Add new forest  
- Domain name: lab.local  
- Set DSRM password  
- Install and reboot

📸 Screenshot: Deployment configuration
<img width="766" height="564" alt="2" src="https://github.com/user-attachments/assets/49b8d1d9-2bd9-47b0-a431-00e7ce0f28ed" />

---

## 🔹 PART 2 — Active Directory Structure

### Create Organizational Units
Active Directory Users and Computers → Domain  
Create OUs:
- HR  
- IT  
- Finance  
- Service Accounts  

📸 Screenshot: OU structure
<img width="759" height="532" alt="3" src="https://github.com/user-attachments/assets/4a496a66-50c6-45f9-92d9-abaff7fd7fcd" />

---

### Create Users
Create users inside OUs:
- helen.hr → HR  
- ian.it → IT  
- fiona.finance → Finance  

📸 Screenshot: Users inside OUs
<img width="752" height="537" alt="4" src="https://github.com/user-attachments/assets/c3b5c8a5-d0d3-4106-806c-36e225ed7af8" />
<img width="753" height="532" alt="5" src="https://github.com/user-attachments/assets/3bbb3db1-6907-4847-aee1-e4ded7023cdc" />
<img width="759" height="534" alt="6" src="https://github.com/user-attachments/assets/059d7c68-fa5f-4df4-a9a0-7053749417a0" />

---

### Populate attributes
User → Properties → Organization  
Set Department:
- HR / IT / Finance  

📸 Screenshot: Department attribute populated
<img width="758" height="512" alt="7" src="https://github.com/user-attachments/assets/16ac5fd1-52a0-40e7-9e5b-0e5af2a2d44c" />
<img width="802" height="563" alt="8" src="https://github.com/user-attachments/assets/31372253-112b-4699-a744-e6399014348b" />
<img width="807" height="550" alt="9" src="https://github.com/user-attachments/assets/66993b93-a695-44d4-86a0-bcfbe8f7a4ed" />

---

## 🔹 PART 3 — Microsoft Entra Setup Account

Create cloud admin account:
Microsoft Entra ID → Users → New user  
Example: syncadmin@tenant.onmicrosoft.com  

Assign role:
Roles → Global Administrator (temporary)

📸 Screenshot: Admin role assignment
<img width="586" height="250" alt="10" src="https://github.com/user-attachments/assets/66248990-b7fa-4c13-a207-786aa909989e" />
<img width="1358" height="604" alt="11" src="https://github.com/user-attachments/assets/b40778b2-ec93-427a-8885-5e6831870c6a" />
<img width="958" height="590" alt="12" src="https://github.com/user-attachments/assets/c8d8c470-9775-4a2e-9e9f-46c80bd47cf8" />

---

## 🔹 PART 4 — AD Sync Service Account

Create in Active Directory:
- Name: svc_EntraConnect  
- Password never expires (lab only)

📸 Screenshot: Service account created
<img width="1065" height="639" alt="15" src="https://github.com/user-attachments/assets/5aef7049-eb64-444e-a5bf-4fc20e848d6a" />
<img width="1231" height="635" alt="16" src="https://github.com/user-attachments/assets/f6252312-c1db-4184-809a-917ff2e36ad0" />

---

## 🔹 PART 5 — Install Microsoft Entra Connect

Download Microsoft Entra Connect on the DC  
Run installer → Express Settings  

Sign in:
- Entra: syncadmin@tenant  
- AD: LAB\Administrator  

Finish installation.

📸 Screenshot: Configuration complete
<img width="939" height="646" alt="17" src="https://github.com/user-attachments/assets/c1056dfd-7ed2-4a23-82c7-4bb6d1cc7370" />
<img width="896" height="640" alt="18" src="https://github.com/user-attachments/assets/fc38ecb5-bb6f-4612-89fa-6c890624fb01" />

---

## 🔹 PART 6 — Verify Synchronization

Microsoft Entra ID → Users  
Verify users appear:
- helen.hr  
- ian.it  
- fiona.finance  

Confirm Department attribute synced.

📸 Screenshots:
- Synced users  
- User properties  
<img width="1365" height="603" alt="19" src="https://github.com/user-attachments/assets/cc9af0f6-3460-4eb3-946a-900d4a836d80" />
<img width="1037" height="549" alt="20" src="https://github.com/user-attachments/assets/c2f1a8ca-d8a0-4f6a-acae-8cbafcc56fd1" />

---

## 🔹 PART 7 — Dynamic Group Automation

Create groups:
Microsoft Entra ID → Groups → New group  
Type: Security  
Membership: Dynamic user  

Groups and rules:

IAM-HR-Users-Dynamic  
(user.department -eq "HR")

IAM-IT-Users-Dynamic  
(user.department -eq "IT")

IAM-Finance-Users-Dynamic  
(user.department -eq "Finance")

📸 Screenshots:
- Dynamic rule configuration  
- Automatic membership  
<img width="738" height="489" alt="21" src="https://github.com/user-attachments/assets/1aef0179-52fc-416a-951a-46c5da050b96" />
<img width="668" height="541" alt="22" src="https://github.com/user-attachments/assets/10eccb25-19b6-441c-9416-0dcaaa9db248" />
<img width="781" height="452" alt="23" src="https://github.com/user-attachments/assets/0ac18c88-cafb-48f5-bf75-50625ff110c4" />
<img width="962" height="473" alt="24" src="https://github.com/user-attachments/assets/6cf661a6-1844-4923-8237-427161c0e961" />
<img width="916" height="475" alt="25" src="https://github.com/user-attachments/assets/0fce5118-a896-4bab-8e15-263afde124e4" />
<img width="1053" height="512" alt="26" src="https://github.com/user-attachments/assets/7479cf9f-d4ac-468b-8390-f98605f00874" />

---

## 🔹 PART 8 — Enterprise Application Access (Optional)

Create enterprise applications:
- IAM-HR-App  
- IAM-IT-App  

Enable: Assignment required  
Assign dynamic groups.

📸 Screenshot: App group assignments
<img width="1348" height="549" alt="27" src="https://github.com/user-attachments/assets/083f1bb4-4b55-4203-8144-fa3deafad9a0" />
<img width="971" height="598" alt="28" src="https://github.com/user-attachments/assets/479b2a6e-8f39-4948-a03c-e27a88c3e726" />
<img width="1359" height="611" alt="29" src="https://github.com/user-attachments/assets/0caae568-a2b5-4d0c-a8f8-a43bc0c8cf7b" />
<img width="999" height="607" alt="30" src="https://github.com/user-attachments/assets/748f6a6b-4960-461e-919f-4c2114968b3f" />

---

# 🔄 Identity Lifecycle Testing (JML)

---

## ✅ Joiner Test — Onboarding

Create user in AD:
- jenny.hr  
- Department: HR  

Verify in Entra:
- User appears  
- HR group membership  
- App access granted  

📸 Screenshots:
- AD user  
- Entra user  
- HR group membership  
<img width="841" height="589" alt="31" src="https://github.com/user-attachments/assets/5a904850-ece7-4c32-ae3b-6a37d84fd5be" />
<img width="1074" height="562" alt="32" src="https://github.com/user-attachments/assets/f03efaa0-f892-4a4c-8613-8784df30af5e" />

---

## 🏅 Mover Test — Role Change

Change helen.hr department:
HR → IT  

Verify in Entra:
- Removed from HR group  
- Added to IT group  
- Access updated  

📸 Screenshots:
- AD attribute change  
- Updated group memberships  
<img width="956" height="632" alt="33" src="https://github.com/user-attachments/assets/f44157e4-1db0-4201-9de8-059483f85343" />
<img width="897" height="620" alt="34" src="https://github.com/user-attachments/assets/4280e711-2fb0-433f-88c5-1798cb86f1c2" />
<img width="1103" height="578" alt="35" src="https://github.com/user-attachments/assets/4f089bcf-82cf-468c-954e-ddad46a585ac" />

---

## 🛡 Leaver Test — Offboarding

Disable user in ADUC.

Verify in Entra:
- Sign-in blocked  

📸 Screenshots:
- AD disabled account  
- Entra sign-in blocked  
<img width="834" height="561" alt="36" src="https://github.com/user-attachments/assets/a38397ac-253e-4eef-82ec-ea5e46183232" />
<img width="732" height="405" alt="37" src="https://github.com/user-attachments/assets/d36b2741-6805-45fa-b69d-2caddafcdba7" />

---

# 📊 Audit & Compliance Evidence

### Directory Audit Logs
Monitoring → Audit logs  
Filter by user  
Capture:
- Group changes  
- Attribute updates  
- Account disablement  

📸 Screenshot: Audit log events
<img width="1059" height="567" alt="39" src="https://github.com/user-attachments/assets/cd4ae29b-acf2-424a-be8c-d0bc2c7b8b45" />

---

### Sign-in Logs
Monitoring → Sign-in logs  
Filter by user  
Capture:
- Successful sign-in (before disable)  
- Failed sign-in (after disable)  

📸 Screenshot: Sign-in log results
<img width="474" height="226" alt="38" src="https://github.com/user-attachments/assets/b01e43ad-3c10-417f-bd9f-91a41a9cf10e" />
<img width="836" height="540" alt="40" src="https://github.com/user-attachments/assets/669e5a45-4afd-4f19-badc-96b28cde5520" />

---

# 🛡 Security Controls Demonstrated

- Hybrid identity architecture  
- Attribute-based access control  
- Dynamic group automation  
- Centralized identity source  
- Secure offboarding  
- Cloud audit logging  

---

# 🧠 Key Skills Demonstrated

- Active Directory administration  
- Microsoft Entra Connect deployment  
- Hybrid identity engineering  
- Attribute synchronization  
- IAM automation  
- Identity lifecycle management  
- Audit and investigation workflows  

---

# 📌 Lessons Learned

- Active Directory remains the enterprise identity source  
- Attribute quality is critical for automation  
- Hybrid sync introduces lifecycle delays that must be monitored  
- Offboarding is the highest-risk identity event  
- Audit and sign-in logs validate IAM controls  
