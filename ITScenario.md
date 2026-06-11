# IT Scenarios in AD


## 🧔 User Management 

### Delegating User and Group 

Delegating Tier 2

### 🛡️ Administrative Delegation & Tiering Logic

To maintain the "Principle of Least Privilege," I have implemented a tiered delegation model. Access is strictly separated to prevent "Lateral Movement" (a Tier 2 user gaining Tier 0 power).

| Tier | Object Category | Delegated Group | Scope of Control | 
| :--- | :--- | :--- | :--- |
| **Tier 0** | Domain Controllers | **Domain Admins** | Full Forest Control |
| **Tier 1** |  Servers | **T1-ServerAdmins** | Local Admin / Service Mngmt | 
| **Tier 2** | End-User Assets | **T2-HelpdeskSupport** | User Management| 


> **Security Note:** In my simulation, Tier 2 accounts are explicitly denied the ability to log in to Tier 1 or Tier 0 assets via GPO. This ensures that even if a technician's account is compromised, the "Core" of the network remains secure.



### 🎟️ Ticket-0124 : Chris Redfield - IM LOCKED OUT PLEASE AHHHHHH

"I've been locked out of my account after too many attempts, please help. I need to do some work"


Actions Taken:

- Verify the User - We must verify the user to make sure that the user requesting help is who they claim to be to prevent unauthorized access.


- Go to ADUC 

<img width="748" height="156" alt="image" src="https://github.com/user-attachments/assets/c500c8dd-c278-431f-aaf0-cfd8729fb9bb" />


- Go the user 

<img width="718" height="510" alt="image" src="https://github.com/user-attachments/assets/b63c43fe-6597-47af-95fc-868de8acd4ec" />


- Unlock Account by:

  - Properties > Account > Check Unlock Account

<img width="407" height="386" alt="image" src="https://github.com/user-attachments/assets/fa803a39-d873-4571-ace6-8a1504e84f4d" />

<img width="408" height="551" alt="image" src="https://github.com/user-attachments/assets/9741fa9f-01a7-490f-82a9-db9eb10ca7db" />



### 🎟️ Ticket-0125: Hunk - I forgot my password

"I've forgotten my password please help!"

Action Taken:

- Verify the User

- Go to ADUC

- Go to the User

- Reset the Passoword by:

  - All tasks > Reset Password > Give temporary password > check "user must change password at next logon"

<img width="998" height="684" alt="image" src="https://github.com/user-attachments/assets/85d6cde3-5aa9-4167-9a05-1a9d0e9e5a89" />

<img width="380" height="256" alt="image" src="https://github.com/user-attachments/assets/a9a1450a-9f5e-4f0e-a681-382a4e67aa56" />



### 🎟️ Ticket-0127 - Onboarding 

"5 new hitres is joining the Dallas region. Please provision the accounts with standard user access."

Actions Taken:

- Created a script to put all 5 hires in the right location via PowerShell ISE

<img width="666" height="412" alt="image" src="https://github.com/user-attachments/assets/638a7146-db07-4889-a192-8e3fb45234f3" />


<img width="539" height="128" alt="image" src="https://github.com/user-attachments/assets/d76f5524-6483-4473-9040-607319fa1ebc" />


<img width="536" height="433" alt="image" src="https://github.com/user-attachments/assets/7f777068-263a-4540-998d-a1f27820d19e" />



### 🎟️ Ticket-0127 - Department Move

"Leon Kennedy got promoted to from a DAL Helpdesk postion to a T1 Atlanta admin.


Manual Way (through ADUC)

- Go to ADUC > Tier 2 > T2-HelpDeskAdmins > DAL-Helpdesk
- Right click Leon Kennedy > Move
- Select Tier 1 > T1-Admins > T1-DalAdmin


Security Group

- Click Leon > Properties > Member of > add T1-Server Admin



# 🛡️ Security Management 



### 🎟️ Ticket-25819 - Compromised LOCK EM OUT!!

"There has been reports of multiple suspicious login attempts from an unknown IP address to Leon Kennedy's account (leonken2026-01) please address this issue ASAP" 

Actions Taken:

- Isolate the User:
  - Used EDR software to perform a network isolation on the infected host. This severed the connection to the internet and internal servers, effectively stopping lateral movement and preventing the attacker from communicating with their command server.


- Revoke Access:
  - Executed a full session purge to remove the attacker's foothold:
  - Disabled the AD account to prevent any new authentication attempts.
  - Forced a remote logoff (shutdown /r /f) to clear the attacker's active desktop session and cached credentials.
  - Revoked cloud sessions in Entra ID to instantly kill active connections to email, Teams, and VPN.


- Reset Password:
  - Performed a password reset to reclaim the identity. Explicitly unchecked "User must change password at next logon" to prevent the attacker from intercepting the reset. 


### 🎟️ Ticket-45132 - Unauthorized Privilleges

"There has been unauthorized privileged given to ADA wong, please resolve this immediately"


<img width="379" height="167" alt="image" src="https://github.com/user-attachments/assets/5068f27b-6aff-492f-9109-6a79258ba691" />


Actions taken:
- Isolate the User to cut network connection to prevent access from any internal server or the internet
- Disable the account qu


<img width="615" height="344" alt="image" src="https://github.com/user-attachments/assets/2a834513-f668-4ab3-8e66-dddc97fcc9c5" />

- Force logoff:


  <img width="612" height="43" alt="image" src="https://github.com/user-attachments/assets/8c037fd3-517d-4951-8a5a-02c5e2d7dc3b" />

OR

<img width="508" height="21" alt="image" src="https://github.com/user-attachments/assets/84fe1f6c-4801-459e-ab9b-a494b4b34d61" />


- Remove Ada wong from the Domain Admins group


  <img width="407" height="405" alt="image" src="https://github.com/user-attachments/assets/83abed0d-6cf8-480a-bbc8-04a93b907099" />


- Then Reset Password





