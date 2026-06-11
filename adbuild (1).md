# 🏢 ILoveBreadrolls Inc. – Enterprise Active Directory Infrastructure Project

---

📌 Project Overview 
This Active Directory will be designed and structured with an attempt of a simplified tiered structure to reflect real-world organizational security practices.

While this AD deployement will incorporate my knowing of a simplified tiered structure (which is not so proficient at this point of time), the primary focus is to demonstrate the capability of practical IT support tasks, from user management to account troubleshooting.

This Active Directory environment is designed using a simplified tiered structure to reflect real-world organizational and security practices.



---

🏢 Company Profile

Industry: Food

Headquarters: Dallas, Texas

Branch Office: Atlanta, Georgia

Workforce Model: Hybrid (On-Premises + Remote)

---

🎯Objectives

 

---

## Deploy Active Directory Domain Service

Objective: Set up an Active Directory Domain Services (AD DS) environment for a 50-user office, including domain controller configuration and user/group management.

Tools & Environment Used:
- Oracle Virtual Box
- Windows Server 2022



### #1 - Domain Controller must use a static IP address 

- A Domain Controller must maintain a static IP address so Active Directory Domain Services and DNS can consistently communicate with devices on the network. If the IP address changes (enabled DHCP), it can cause authentication failures and network connectivity issues.


But first change the defualt computer name 

---

1. Go to "Configure this local server"

<img width="388" height="253" alt="image" src="https://github.com/user-attachments/assets/8f23b7f2-e0e6-4e02-9626-b77c81f4f4a0" />


2. "Computer Name"


<img width="279" height="45" alt="image" src="https://github.com/user-attachments/assets/ca5c6845-f67b-4185-b338-1c1d9177fa32" />


3. Click "Change"


<img width="400" height="467" alt="image" src="https://github.com/user-attachments/assets/4de09f00-4e3a-4a10-a44d-b464167d9a66" />


4. Change Computer Name to DC1


<img width="314" height="387" alt="image" src="https://github.com/user-attachments/assets/66211ce0-ddc6-4951-87a1-767ee8718834" />

- It needs to restart to make the changes but were going to restart after implementing the static IP


Now to assign a static IPO

1. Go to ethernet


<img width="460" height="268" alt="image" src="https://github.com/user-attachments/assets/39f31b11-d52e-4333-affa-822f30a91a40" />


2. Right click and go to "Properties"


<img width="338" height="160" alt="image" src="https://github.com/user-attachments/assets/ee8bb062-6221-4b63-b796-f1ea006c8df7" />


3. Click on "Internet Protocol Version 4" then assign the following

<img width="397" height="456" alt="image" src="https://github.com/user-attachments/assets/2dda0a7d-d7ae-4539-adf2-8b1ebd39c286" />


<img width="389" height="395" alt="image" src="https://github.com/user-attachments/assets/205d860f-470c-4f09-a308-cc644819e8b8" />


5. Now restart (After restart you can verify by going to Windows Powershell and put in IPCONFIG)

---

### #2 Add Roles and Features

- There will be minimal amount of roles and features. While, there are various amounts that might seem useful. However, it is important to reduce as much of the attack surface of the domain controller. Only installing what is necessary that are required for Active Directoy management.


1st - Click on "add roles and features"

Before You Begin -> Next

Installation Type -> Click "Role-based or feature-based installation"

Server Selection -> It should only be DC1

Server Roles ->
1. Active Directory Domain Services - stores the directory database of users, computers, groups. Also handles authentication and authorization. Allows admins to manage the identities across network.

2. DNS Server -> 

<img width="563" height="470" alt="image" src="https://github.com/user-attachments/assets/6a2fd331-e29c-493a-a270-078624f43311" />


Features ->
1. .NET Framework 4.8 features 
2. Group Policy Management
3. Remote Server Administration Tools


<img width="786" height="559" alt="image" src="https://github.com/user-attachments/assets/2b822d70-c714-4ead-875e-0095b7f5dd68" />


### #3 Promote server to a Domain Controller


<img width="362" height="320" alt="image" src="https://github.com/user-attachments/assets/dc4a4948-6ea1-43b6-b9ab-b57f88ee5ec8" />


<img width="752" height="552" alt="image" src="https://github.com/user-attachments/assets/1eb332ec-34c0-4a9f-b4ab-cde498ee208a" />

Deployment Configuration:
- Add a new forest - Ilovebreadrolls.local


Domain Controller Options:
- Forest Functional Level: Windows Server 2016 or later
- Domain Functional level: Same
- DNS Server: ☑️
- Gloval Catalog: ☑️
- Set DSRM password


<img width="675" height="422" alt="image" src="https://github.com/user-attachments/assets/4cadbd39-3677-4c25-a491-63ec3f9e3627" />

- Click "Next" till "Install" after install it will restart. After promoting the server to a domain controller, it becomes responsible for managing the domain using AD DS. Windows Server creates the AD database, configures domain structure and integrates with DNS so devices can loate domain services. Also has a SYSVOL folder used for group polict and logon script.


---
  

#  🏗️Implementing AD Structure

The purpose of a tiered administrative model is to separate critical systems, accounts, and administrative privileges into distinct security boundaries to reduce the risk of compromise.

By enforcing strict separation between tiers, administrators and systems are restricted to operating only within their designated level. This prevents credentials from higher-privileged tiers from being exposed to lower, more vulnerable environments.

If a lower-tier system is compromised, the attacker is contained within that tier and cannot easily escalate privileges or move laterally to access higher-tier systems such as servers or domain controllers.

This model helps protect identity infrastructure and minimizes the overall impact of a security breach.



Steps: (I have also gave my self delegate control)
- Open Active Directory Users and Computers
- Right click on the domain (ilovebreadrolls.local) select "New" -> "Organization Unit" (Protect accidental deletion ☑️)

<img width="760" height="523" alt="image" src="https://github.com/user-attachments/assets/5ca19923-77f0-48cc-af84-871bcb509f36" />



### OU Structure
  

<img width="3900" height="1408" alt="Blank diagram" src="https://github.com/user-attachments/assets/97a24978-58a3-43cf-8736-2275d9a752ec" />


Tier 0: The Control Plane (Root of Trust)
- Definition: The highest level of privilege in the Active Directory forest. Tier 0 has direct administrative control over the identity store and all authentication mechanisms.

- Contents: Domain Controllers (DCs), Tier 0 Administrative Accounts, Privileged Admin Workstations (PAWs), and etc. (It must be minimal to reduce attack surface.



Tier 1: Business Assets
- Definition: Contains the high-value resources that drive company operations but do not have authority over the domain's security.

- Contents: File Servers, Database Servers, Application Servers,

We separate Tier 1 from Tier 0 because application servers (especially web-facing ones) have significantly larger attack surfaces. By isolating them, an unpatched application vulnerability cannot be used as a stepping stone to compromise and cause more damage.


Tier 2: User Environment
- Definition: The "Front Line" of the network where 50+ employees perform daily tasks like email, browsing, and document creation.

- Contents: Standard User Accounts, Windows 11 Workstations, Laptops, etc.

As the most exposed tier (vulnerable to phishing and malware), the priority is ensuring that a breach here is "trapped." Through strict GPO-enforced Logon Restrictions, an attacker who gains access to a Tier 2 laptop cannot move laterally into Tier 1 or Tier 0.



### Creating security groups
- Right click "New" -> "Group"


<img width="457" height="342" alt="image" src="https://github.com/user-attachments/assets/6badc633-1a6f-43ed-b5e5-12e8b96a3fb1" />


OR 

(Tier 1 groups example)


<img width="683" height="587" alt="Screenshot 2026-04-02 173556" src="https://github.com/user-attachments/assets/c644e496-cc30-4dd5-991b-61fe11cd39b5" />





🗃️ Tier 0 

- T0--DomainAdmins-GG,Domain,Daily Identity & OU Management,Persistent 
- T0PAWUsers,Hardware,Workstation Logon Authorization,Persistent (Tier 0 Only)


🗃️ Tier 1

- T1-ServerAdmins Tier 1 OUs	Creating and managing server-level AD objects and groups.
- T1-ServerAcc Level	Local Administrator rights on all servers in Dallas/Atlanta.


🗃️ Tier 2

- T2-UserAdmins-DAL/ATL,Regional Users,Password Resets & Unlocks,AD Delegation of Control
- T2-WorkstationMng-DAL/ATL,Regional PCs,Local Administrator Access,GPO Restricted Groups
- T2-VpnUsers-DAL/ATL,Regional Assets,Computer Object Lifecycle,OU Permissions Management




# Group Policies


### Default Domain Policy (Linked to Root: ilovebreadrolls.local)
- Password Complexity: Enforces the use of uppercase, lowercase, numbers, and symbols.

- Password Length: Sets a 12-character minimum to defend against "brute-force" cracking.

- Password Age: Forces a rotation every 90 days but prevents immediate changes within 30 days.

- Account Lockout: Temporarily freezes accounts after multiple failed attempts to stop login guessing.



### T0 Group Policy (Linked to: Tier 1 OU and Tier 2 OU)
- Prevent Lateral Movement: Uses "Deny log on locally/RDS" to block Tier 0 Admins from touching lower-tier machines.




### Domain Controller Policy (Linked to: Domain Controllers OU)

- Logon Banner: Triggers a legal/security warning pop-up before any user can reach the login prompt.



### Tier 1 Policies (Linked to: Tier 1 OU)

- Logon as a Service: Authorizes specific Service Accounts to run background applications without a human session.

- Deny Logon Locally: Blocks Service Accounts from being used by a person to log into a server's desktop.



### Tier 2 Policies (Linked to: Tier 2 OU)

- Restricted Groups: Forces the Tier 2 Helpdesk group into the local "Administrators" group on all PCs.

- User Safeguards: Disables access to "Add or Remove Programs" within the Control Panel.




# Testing GPO 

### Logon Banner ☑️

<img width="1020" height="837" alt="image" src="https://github.com/user-attachments/assets/1c5a88ca-23ec-4e4b-8ea8-536be1f808ea" />





