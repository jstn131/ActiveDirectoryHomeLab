# Group Policy Objects — Step By Step Walkthrough

## Overview
This document provides a step by step walkthrough of creating 
and testing Group Policy Objects in the homelab.local Active 
Directory environment. Four GPOs were created for the Finance 
department covering security, desktop customization, 
restrictions, and drive mapping.

---

## Prerequisites
- Windows Server 2022 VM running as Domain Controller
- Windows 10 CLIENT01 VM joined to homelab.local
- Group Policy Management Console open on Server VM
- Finance OU with Finance-Users and Finance-Computers sub OUs

---

## How To Open Group Policy Management

On the Server VM open Server Manager and click:
```
Tools → Group Policy Management
```

<img width="1023" height="765" alt="Screenshot 2026-04-17 210105" src="https://github.com/user-attachments/assets/233667db-6299-4887-9775-183e130a684e" />


Expand the tree until you can see:
```
Forest: homelab.local
└── Domains
      └── homelab.local
            └── Finance OU
```

---

## GPO 1 — Finance-Security-Policy

### What It Does
Locks the screen automatically after 5 minutes of inactivity 
on any Finance department computer regardless of who is logged in.

**Type:** Computer Configuration
**Applies to:** Finance-Computers sub OU

---

### Step 1 — Create The GPO

Right click the **Finance OU** and select:
```
Create a GPO in this domain and link it here
```

<img width="1024" height="766" alt="Screenshot 2026-04-17 210619" src="https://github.com/user-attachments/assets/f02d6533-4dcf-4f15-bfb7-d0b88b820946" />


Name the GPO:
```
Finance-Security-Policy
```

<img width="1017" height="768" alt="Screenshot 2026-04-17 211140" src="https://github.com/user-attachments/assets/fa0a8fed-e743-4608-9c18-f693a58c7481" />


---

### Step 2 — Edit The GPO

Right click **Finance-Security-Policy** and select **Edit.**

<img width="1016" height="764" alt="Screenshot 2026-04-17 212023" src="https://github.com/user-attachments/assets/9ee7d7a4-c400-4476-a5d4-1b61f47257b1" />


Navigate to:
```
Computer Configuration
→ Policies
→ Windows Settings
→ Security Settings
→ Local Policies
→ Security Options
```

<img width="1022" height="761" alt="Screenshot 2026-04-17 213537" src="https://github.com/user-attachments/assets/c2d8bd94-596a-4383-823e-df36056258f1" />


---

### Step 3 — Configure Machine Inactivity Limit

Scroll through Security Options and find:
```
Machine Inactivity Limit
```

Double click it and:
- Check **Define this policy setting**
- Set value to `300` (300 seconds = 5 minutes)
- Click **Apply** → **OK**

<img width="1013" height="762" alt="Screenshot 2026-04-17 214222" src="https://github.com/user-attachments/assets/639c8949-cc1f-4edb-938e-d60a1452be30" />


---

### Step 4 — Move CLIENT01 To Finance-Computers

For Computer Configuration policies to apply the computer 
account must be in the correct OU.

In my case, CLIENT01 was outside the Finance OU. I moved it
into the Finance OU but did not like the fact that users and
computers were going to be mixed up. So I created sub-OU's, one
for finance users, and the other for finance computers.

In Active Directory Users and Computers:
- Find CLIENT01 in the Computers container
- Right click → **Move** → Select **Finance-Computers**

---

### Step 5 — Test The Policy

On CLIENT01 open Command Prompt as Administrator and run:

```cmd
gpupdate /force
```

Leave CLIENT01 completely idle for 5 minutes without 
touching the mouse or keyboard.

---

## GPO 2 — Finance-Desktop-Policy

### What It Does
Forces a specific desktop wallpaper on all Finance department 
user accounts. Follows the user to any machine they log into.

**Type:** User Configuration
**Applies to:** Finance-Users sub OU

---

### Step 1 — Create The GPO

Right click **Finance OU** → **Create a GPO in this domain 
and link it here**

Name it:
```
Finance-Desktop-Policy
```
<img width="1020" height="766" alt="Screenshot 2026-04-18 142142" src="https://github.com/user-attachments/assets/5ba6803b-06b5-457c-b35a-6d81c9b26560" />


---

### Step 2 — Navigate To Desktop Wallpaper Setting

Right click **Finance-Desktop-Policy** → **Edit**

Navigate to:
```
User Configuration
→ Policies
→ Administrative Templates
→ Desktop
→ Desktop
```

<img width="1023" height="764" alt="Screenshot 2026-04-18 142327" src="https://github.com/user-attachments/assets/4ae801b3-d589-4d30-a7d1-85be9ef3e1fa" />


Double click **Desktop Wallpaper**

<img width="1023" height="768" alt="Screenshot 2026-04-18 142754" src="https://github.com/user-attachments/assets/e043755b-9cfb-41aa-b64b-103f97354dd2" />


---

### Step 3 — Configure The Wallpaper

Note: For the wallpaper name, it can be any file path, as long as
the workstation HAS that image stored in it. For this homelab, I simply
used the simple Windows defualt images.

- Click **Enabled**
- Set Wallpaper Name to:
```
C:\Windows\Web\Wallpaper\Theme2\img7.jpg
```
- Set Wallpaper Style to: **Fill**
- Click **Apply** → **OK**

> **Important:** The file path must exist on the target 
> machine. An incorrect path results in a black desktop.

---

### Step 4 — Test The Policy

On CLIENT01 run:
```cmd
gpupdate /force
```

Then **log out and log back in** as john.smith.
User Configuration policies apply on login.

<img width="1021" height="762" alt="Screenshot 2026-04-18 144457" src="https://github.com/user-attachments/assets/23ef4d63-0d28-4994-aa7d-b61c28a0ae0f" />


---

## GPO 3 — Finance-Restrictions-Policy

### What It Does
Prevents Finance department users from opening Control Panel 
or Windows Settings. Enforces a security standard preventing 
users from changing system configurations.

**Type:** User Configuration
**Applies to:** Finance-Users sub OU

---

### Step 1 — Create The GPO

Right click **Finance OU** → **Create a GPO in this domain 
and link it here**

Name it:
```
Finance-Restrictions-Policy
```

---

### Step 2 — Navigate To Control Panel Setting

Right click **Finance-Restrictions-Policy** → **Edit**

Navigate to:
```
User Configuration
→ Policies
→ Administrative Templates
→ Control Panel
```

<img width="1020" height="764" alt="Screenshot 2026-04-18 145648" src="https://github.com/user-attachments/assets/8a121c0c-223a-4755-b538-e28cd2dc6a42" />


Double click:
```
Prohibit access to Control Panel and PC Settings
```

---

### Step 3 — Enable The Setting

- Click **Enabled**
- Click **Apply** → **OK**

> This setting has no additional options — simply enabling 
> it activates the restriction immediately.

---

### Step 4 — Test The Policy

On CLIENT01 run:
```cmd
gpupdate /force
```

Log out and log back in as john.smith then try opening 
Control Panel or Settings.

<img width="1019" height="769" alt="Screenshot 2026-04-18 150510" src="https://github.com/user-attachments/assets/f129ea82-8c8f-4c0b-8a03-0a402faa1da7" />

---

## GPO 4 — Finance-DriveMaps-Policy

### What It Does
Automatically maps the Finance shared folder as F: drive 
for all Finance users on login. Eliminates the need for 
users to manually navigate to shared folders using UNC paths.

**Type:** User Configuration → Preferences  
**Applies to:** Finance-Users sub OU

> **Note:** Drive mapping is configured under Preferences 
> not Policies because it is a convenience setting rather 
> than a security requirement.

---

### Step 1 — Create The Shared Folder On The Server

On the Server VM open **Server Manager.**

On the left panel click **File and Storage Services** then 
select **Shares.** You will see existing shares like NETLOGON 
listed here.

<img width="1015" height="764" alt="Screenshot 2026-04-18 160849" src="https://github.com/user-attachments/assets/9a167fb1-8ab9-41e7-8d60-6f89358abe2e" />


Click **Tasks** in the top right and select **New Share.**

<img width="1017" height="765" alt="Screenshot 2026-04-18 160910" src="https://github.com/user-attachments/assets/fa7ef834-4902-44be-8b07-0bae0d4989a1" />


A wizard will open showing share profile options. Select:
```
SMB Share - Quick
```

This gives you everything needed for a basic file share 
without unnecessary complexity.

---

### Step 2 — Select Share Location

On the Share Location screen scroll to the bottom and 
select **Type a custom path.** Click **Browse** and 
navigate to the folder you want to share. In my case,
Finance-Folder

<img width="1018" height="768" alt="Screenshot 2026-04-18 161208" src="https://github.com/user-attachments/assets/f77f0d1c-ea08-454c-8b2a-ec3dea313d1a" />

> You can also create a new folder directly from this 
> browse dialog if the folder doesn't exist yet.

Click **Next.**

---

### Step 3 — Configure Share Settings

On the settings screen make sure to check:

```
✅ Enable Access-Based Enumeration
```

**What is Access-Based Enumeration?**
This setting ensures users only see files and folders they 
have permission to access. If a user does not have at least 
read-only permission to a folder Windows hides it completely 
from their view — they won't even know it exists.

Without this setting users might see folders they can't 
open which is confusing and a potential security concern.

<img width="1025" height="771" alt="Screenshot 2026-04-18 161323" src="https://github.com/user-attachments/assets/c4f347e6-102b-4906-9b07-122340002924" />

---

### Step 4 — Configure Permissions

On the Permissions screen click **Customize Permissions.**

**Step 4a — Disable Inheritance**

First click **Disable Inheritance.**

> **Why disable inheritance?**
> The Finance-Folder was created inside the C: drive which 
> means it inherits permissions from its parent folder. 
> This gives access to everyone who has access to C: — 
> which is too broad. Disabling inheritance breaks this 
> connection so we can set our own specific permissions 
> from scratch.

When prompted choose **Convert inherited permissions into 
explicit permissions** so we can then remove unwanted ones.

<img width="1018" height="764" alt="Screenshot 2026-04-18 161549" src="https://github.com/user-attachments/assets/a40a935c-0961-4631-a6f2-29290fe7f91b" />


**Step 4b — Remove Users Group**

You will see several permission entries including a 
**Users** group. Remove this immediately.

> **Why remove Users?**
> The Users group includes every single account in the 
> domain. Leaving it means everyone can access the 
> Finance folder — which defeats the entire purpose of 
> creating a department specific share.

Select **Users** → click **Remove**

**Step 4c — Add Finance-Team Security Group**

<img width="1016" height="767" alt="Screenshot 2026-04-18 161708" src="https://github.com/user-attachments/assets/67bcbb10-52c5-4197-9bc1-86d631141066" />


Click **Add** → **Select a Principal**

Search for:
```
Finance-Team
```

Click **Check Names** to verify it resolves correctly 
then click **OK.**

Set the following permissions:

```
✅ Read
✅ Write
❌ Full Control — leave unchecked
```

<img width="1018" height="765" alt="Screenshot 2026-04-18 161938" src="https://github.com/user-attachments/assets/e70d1216-9396-42f6-8cd5-0ab341dcaec1" />


> **Why not Full Control?**
> Full Control allows users to delete files and change 
> permissions — too much access for regular Finance 
> employees. Read and Write allows them to open, create, 
> and edit files which is all they need.

Click **Apply** → **OK**

---

### Step 5 — Complete The Share Creation

Click **Next** through the remaining screens and click 
**Create.**

Your Finance-Folder is now a shared network folder 
accessible only by members of the Finance-Team 
Security Group.

---

### Step 6 — Verify The Share Works

On CLIENT01 press:
```
Win + R
```

Type the UNC path:
```
\\WIN-1FGEG1FMJLD\Finance-Folder
```


If the Finance-Folder opens successfully the share is 
working correctly and only Finance-Team members can access it.

---

### Step 7 — Create The GPO

Back on the Server VM in Group Policy Management:

Right click **Finance OU** → **Create a GPO in this domain 
and link it here**

Name it:
```
Finance-DriveMaps-Policy
```

---

### Step 8 — Navigate To Drive Maps

Right click **Finance-DriveMaps-Policy** → **Edit**

Navigate to:
```
User Configuration
→ Preferences
→ Windows Settings
→ Drive Maps
```

<img width="1012" height="766" alt="Screenshot 2026-04-18 205447" src="https://github.com/user-attachments/assets/00f29d0a-06eb-482e-94df-806be0c8e1ed" />


Right click in the empty blue area and select:
```
New → Mapped Drive
```
<img width="1017" height="775" alt="Screenshot 2026-04-18 205707" src="https://github.com/user-attachments/assets/204a9ca1-f88f-44cf-9a26-c591ea663521" />

---

### Step 9 — Configure The Drive Mapping

Fill in the General tab:

```
Action:        Update
Location:      \\ ServerName \ Foldername [\\WIN-1FGEG1FMJLD\Finance-Folder]
Drive Letter:  F:
Label:         Finance Drive
Reconnect:     ✅ Checked
```

<img width="1012" height="766" alt="Screenshot 2026-04-18 210048" src="https://github.com/user-attachments/assets/bac53cf1-ebae-4b9a-99f0-9042db741af2" />


**What each setting means:**

| Setting | Purpose |
|---|---|
| **Action: Update** | Creates drive if missing, updates if exists |
| **Location** | UNC path to the shared folder on the server |
| **Drive Letter F:** | F for Finance — easy to remember |
| **Label** | What users see in File Explorer |
| **Reconnect** | Remaps automatically on every login |

Click **Apply** → **OK**

---

### Step 10 — Test The Policy

On CLIENT01 open Command Prompt as Administrator and run:

```cmd
gpupdate /force
```

Then **log out and log back in** as john.smith.

Open **File Explorer** and look under **This PC.**

<img width="1022" height="721" alt="Screenshot 2026-04-18 210544" src="https://github.com/user-attachments/assets/9cad781d-71ea-4c12-9eb7-f2e373ee43fe" />


The F: Finance Drive should appear automatically without 
john.smith having to do anything. This is exactly how 
shared drives work in real enterprise environments — 
employees see their department drive automatically on 
every login.

---

## Verifying All GPOs Are Applied

On CLIENT01 open Command Prompt as Administrator and run:

```cmd
gpresult /r
```

Under **Applied Group Policy Objects** you should see 
all four GPOs listed:

```
Finance-Security-Policy
Finance-Desktop-Policy
Finance-Restrictions-Policy
Finance-DriveMaps-Policy
```
---

## Key Commands Reference

| Command | Purpose |
|---|---|
| `gpupdate /force` | Forces immediate GPO refresh from DC |
| `gpresult /r` | Shows all currently applied GPOs |
| `gpresult /r /scope computer` | Shows computer GPOs only — run as admin |
| `gpresult /r /scope user` | Shows user GPOs only |
