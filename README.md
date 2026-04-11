# Active Directory Home Lab

## Overview 
> Built as part of a personal IT portfolio to prepare
> for helpdesk and IT support roles

This project is a home lab I built to gain hands-on experience with 
Active Directory and practice common helpdesk tasks in a real enterprise 
environment. Using Windows Server 2022 in VirtualBox, I set up a Domain 
Controller, created a domain called homelab.local, and simulated a 
real company IT environment from scratch.

In this lab I created Organizational Units, user accounts, and Security 
Groups across multiple departments. I also practiced core helpdesk 
tasks including password resets, account unlocks, disabling terminated 
employee accounts, and transferring users between departments while 
preventing Privilege Creep by removing old group memberships during 
transfers.

## Technologies Used
- Windows Server 2022
- Oracle VirtualBox
- Active Directory Domain Services (AD DS)
- DNS Server
- Group Policy Management
- Active Directory Users and Computers (ADUC)
- Windows 10 Pro
- TCP/IPv4 Static IP Configuration

## Lab Environment

| Component | Details |
|---|---|
| **Host Machine** | Windows 11, 16GB RAM, 1TB SSD |
| **Hypervisor** | Oracle VirtualBox 7.2.6 |
| **Server VM** | Windows Server 2022 Standard Evaluation |
| **Server RAM** | 4GB allocated |
| **Server Storage** | 50GB VDI |
| **Domain Name** | homelab.local |
| **Domain Controller** | WIN-1FGEG1FMJLD |
| **Forest Functional Level** | Windows Server 2016 |

## What I Built

### Organizational Units
Created three OUs to simulate a real company department structure:
- `Finance` — containing Finance department employees
- `IT` — containing IT department employees
- `HR` — containing HR department employees

### User Accounts
Created eight domain user accounts across all three departments 
with temporary passwords and forced password change on first logon:

| User | Department | Username |
|---|---|---|
| John Smith | Finance | john.smith |
| Sarah Connor | Finance → HR | sarah.connor |
| Harvey Specter | Finance | harvey.specter |
| Mike Johnson | IT | mike.johnson |
| Justin Fox | IT | justin.fox |
| Michael Ross | IT | michael.ross |
| Karen Davis | HR | karen.davis |
| Rachel Zane | HR | rachel.zane |

### Security Groups
Created three Security Groups to control access to department resources:
- `Finance-Team` — Harvey Specter, John Smith
- `IT-Team` — Mike Johnson, Justin Fox, Michael Ross
- `HR-Team` — Karen Davis, Rachel Zane, Sarah Connor

## Helpdesk Tasks Simulated

### 1. User Onboarding
Created new domain user accounts inside the correct Organizational Unit
with a temporary password and enforced password change on first logon,
following standard onboarding procedure.

### 2. Password Reset
Simulated a user forgetting their password. Located the user account 
in Active Directory Users and Computers, right clicked and selected 
Reset Password, assigned a temporary password, and enforced password 
change on next logon.

### 3. Account Unlock
Identified a locked user account in Active Directory. Navigated to the 
user's Account tab in Properties and unchecked the locked account 
checkbox to restore access immediately.

### 4. Account Disable — Employee Termination
Simulated an employee termination request from HR. Disabled the user 
account immediately to prevent access while preserving all account data 
in accordance with the standard 90-day disable before delete policy.

### 5. Department Transfer
Transferred a user from the Finance department to HR by:
- Moving the user account to the HR Organizational Unit
- Removing the user from the Finance-Team Security Group
- Adding the user to the HR-Team Security Group
- Following Privilege Creep prevention best practices by removing 
  all old group memberships before completing the transfer

### 6. Domain Join — New Computer Setup
Set up a Windows 10 Pro VM, configured static IP addressing, 
and joined the machine to the homelab.local domain. Verified 
successful domain authentication by logging in as john.smith 
and confirming domain controller WIN-1FGEG1FMJLD as the 
logon server using whoami and echo %logonserver% commands.

## What I Learned

- How to install and configure Active Directory Domain Services (AD DS)
  on Windows Server 2022
- How to promote a Windows Server to a Domain Controller and create 
  a new forest and domain from scratch
- The difference between Organizational Units and Security Groups and 
  when to use each one
- How to manage user accounts including creation, password resets, 
  account unlocks, and disabling accounts
- How to prevent Privilege Creep by properly managing Security Group 
  memberships during department transfers
- The difference between disabling and deleting accounts and why the 
  90-day disable before delete policy exists
- The difference between on-premise Active Directory and Azure Active 
  Directory and when each is used in modern enterprise environments
- How to troubleshoot VM boot issues and work with ISO files and 
  virtualization software
- How to configure static IP addresses and DNS settings for domain connectivity
- How to join a Windows 10 Pro machine to an Active Directory domain
- How to verify domain authentication using whoami and logonserver commands

## Screenshots

### Active Directory Users and Computers — Domain Structure
*Screenshot showing homelab.local domain with Finance, IT, and HR 
Organizational Units*
<img width="1016" height="767" alt="Screenshot 2026-04-05 135722" src="https://github.com/user-attachments/assets/c4d9bdd3-c7f5-4381-9425-d666a622c027" />


### Finance OU — Users and Security Group
*Screenshot showing Finance department users and Finance-Team 
Security Group*
<img width="758" height="529" alt="Screenshot 2026-04-05 141943" src="https://github.com/user-attachments/assets/f0497f70-5dcd-45e1-b072-c9f479010874" />


### IT OU — Users and Security Group
*Screenshot showing IT department users and IT-Team Security Group*
<img width="751" height="530" alt="Screenshot 2026-04-05 141902" src="https://github.com/user-attachments/assets/6d49452d-6da5-457a-a87f-dccb6660e04a" />


### HR OU — Users and Security Group
*Screenshot showing HR department users and HR-Team Security Group*
<img width="758" height="532" alt="Screenshot 2026-04-05 141930" src="https://github.com/user-attachments/assets/5c45614b-625c-42fc-b3a8-c931324c05d1" />


### Security Group Members — Finance-Team
*Screenshot showing Finance-Team membership*
<img width="751" height="528" alt="Screenshot 2026-04-05 150056" src="https://github.com/user-attachments/assets/0d6ed5f3-694b-4f7b-96a4-d377053a0fc0" />


### Password Reset
*Screenshot showing password reset steps and dialog for John Smith*
<img width="754" height="526" alt="Screenshot 2026-04-05 144247" src="https://github.com/user-attachments/assets/4535928c-dfcb-40ea-8920-d519d4ec9d62" />

<img width="1029" height="773" alt="Screenshot 2026-04-05 144425" src="https://github.com/user-attachments/assets/d6349487-6289-438d-ac8b-2a1ecb77b88d" />


<img width="750" height="528" alt="Screenshot 2026-04-05 144841" src="https://github.com/user-attachments/assets/d246bd39-06d2-43f8-b502-5b45365135b7" />


### Account Disable — Harvey Specter
*Screenshot showing disabled account confirmation*
<img width="750" height="522" alt="Screenshot 2026-04-05 145335" src="https://github.com/user-attachments/assets/f35010cc-9083-47de-85ef-30699ae21f84" />

### Domain Join Verification — john.smith
*Screenshot showing successful domain authentication via whoami 
and logonserver commands confirming CLIENT01 joined homelab.local*
<img width="966" height="362" alt="Screenshot 2026-04-11 145820" src="https://github.com/user-attachments/assets/c7cd4110-3b7a-4577-8560-898068f85319" />



## Lab Architecture

### Network topology
| Machine | OS | IP Address | Role |
|---|---|---|---|
| WIN-1FGEG1FMJLD | Windows Server 2022 | 192.168.10.1 | Domain Controller / DNS |
| CLIENT01 | Windows 10 Pro | 192.168.10.2 | Domain joined client |

### Network
Both machines connected via VirtualBox Internal Network (HomeLab-Network)

## Author
**Justin Hernandez**  
CIS Student — Cal Poly Pomona  
Information Technology Intern @ LA-Tech.org  
[[GitHub Profile](https://github.com/jstn131)]  
[[LinkedIn Profile](www.linkedin.com/in/jstn131)]
