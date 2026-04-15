# Active Directory Home Lab — Installation Guide

## Overview
This guide documents the complete installation and setup process 
for the Active Directory Home Lab. This includes setting up the 
virtualization environment, installing Windows Server 2022, 
promoting it to a Domain Controller, and joining a Windows 10 
client machine to the domain.

---

## Prerequisites

| Requirement | Minimum | Recommended |
|---|---|---|
| **RAM** | 8GB | 16GB |
| **Storage** | 80GB free | 100GB+ free |
| **OS** | Windows 10/11 | Windows 11 |
| **Processor** | Dual core | Quad core |

---

## Software Required

| Software | Purpose | Download |
|---|---|---|
| Oracle VirtualBox 7.2.6 | Hypervisor for running VMs | virtualbox.org |
| Windows Server 2022 ISO | Domain Controller OS | Microsoft Evaluation Center |
| Windows 10 Pro ISO | Client machine OS | Microsoft |

---

## Phase 1 — Setting Up VirtualBox

**Step 1 — Download and install VirtualBox**

Download VirtualBox from https://www.virtualbox.org and install 
with all default settings. Also download and install the 
**VirtualBox Extension Pack** from the same page for improved 
VM performance.

**Step 2 — Verify virtualization is enabled**

Open Task Manager → Performance → CPU and confirm 
**Virtualization: Enabled** is showing. If disabled it must 
be enabled in your BIOS before proceeding.

**Step 3 — Organize your lab folder structure**

Create the following folder structure to keep lab files organized:

```
C:\Homelab\
├── ISOs\
│     ├── SERVER_EVAL_x64FRE_en-us.iso
│     └── Windows.iso
├── Windows Server 2022\
└── Windows Clients\
```

> **Important:** Do not store ISO files in OneDrive folders. 
> VirtualBox cannot reliably read ISO files from OneDrive paths 
> and will fail to boot. Always store ISOs directly on your 
> local C: drive.

---

## Phase 2 — Downloading the ISOs

### Windows Server 2022 ISO

Download from Microsoft's Evaluation Center:
👉 https://www.microsoft.com/en-us/evalcenter/evaluate-windows-server-2022

- Click **Download the ISO** — not any other ISO link on the page
- Fill out the registration form
- Select **64-bit** English edition
- Save directly to `C:\Homelab\ISOs\`
- File name: `SERVER_EVAL_x64FRE_en-us.iso`
- File size: approximately 4.7GB

> **Warning:** The evaluation center page contains multiple ISO 
> download links. Make sure you click **"Download the ISO"** from 
> the main evaluation section — NOT the supplementary Languages 
> and Features ISO link which contains no operating system.

### Windows 10 Pro ISO

Download using Microsoft's Media Creation Tool:
👉 https://www.microsoft.com/en-us/software-download/windows10

- Click **Download Now** under Create Windows 10 installation media
- Run the tool and select **Create installation media for another PC**
- Select **ISO file** when prompted
- Save directly to `C:\Homelab\ISOs\`
- File size: approximately 4.7GB

> **Note:** Windows 10 **Pro** edition is required for domain 
> joining. Windows 10 Home does not support Active Directory 
> domain join and cannot be used as a domain client machine.

---

## Phase 3 — Creating the Windows Server 2022 VM

Open VirtualBox and click **New.** Configure as follows:

**VM Settings:**

| Setting | Value |
|---|---|
| **Name** | Windows Server 2022 |
| **Folder** | C:\Homelab\ |
| **ISO Image** | C:\Homelab\ISOs\SERVER_EVAL_x64FRE_en-us.iso |
| **Type** | Microsoft Windows |
| **Version** | Windows Server 2022 (64-bit) |
| **Unattended Install** | Unchecked |

**Hardware Settings:**

| Setting | Value |
|---|---|
| **RAM** | 4096 MB |
| **CPUs** | 2 |
| **Use EFI** | Unchecked |

**Storage Settings:**

| Setting | Value |
|---|---|
| **Disk Size** | 50GB |
| **Format** | VDI |
| **Pre-allocate** | Unchecked |

**Boot Order (System Settings):**

| Device | Enabled | Order |
|---|---|---|
| Floppy | ❌ | — |
| Optical | ✅ | 1st |
| Hard Disk | ✅ | 2nd |
| Network | ❌ | — |

---

## Phase 4 — Installing Windows Server 2022

Boot the VM and follow the installation wizard:

| Step | Selection |
|---|---|
| **Language** | English (United States) |
| **Edition** | Windows Server 2022 Standard Evaluation (Desktop Experience) |
| **License** | Accept |
| **Installation Type** | Custom |
| **Drive** | Drive 0 Unallocated Space (50GB) |

> **Important:** Always select the **Desktop Experience** edition. 
> The non-Desktop Experience edition installs with no graphical 
> interface — command line only — which is significantly harder 
> to learn on as a beginner.

**After installation completes:**
- Set Administrator password when prompted
- Must meet complexity requirements — uppercase, lowercase, 
  number, and special character
- Example: `admin@123`
- Log in using **Input → Keyboard → Insert Ctrl+Alt+Delete** 
  in VirtualBox

---

## Phase 5 — Installing Active Directory Domain Services

Once logged into Windows Server open **Server Manager** and:

1. Click **Add Roles and Features**
2. Select **Role-based or feature-based installation**
3. Select your server from the server pool
4. Check **Active Directory Domain Services**
5. Click **Add Features** when prompted
6. Click through Features and AD DS information screens
7. Click **Install** on the Confirmation screen
8. Wait for installation to complete

---

## Phase 6 — Promoting The Server To A Domain Controller

After AD DS installs click **"Promote this server to a domain 
controller"** in the results screen or via the yellow flag in 
Server Manager.

Configure the promotion wizard as follows:

| Setting | Value |
|---|---|
| **Deployment Operation** | Add a new forest |
| **Root Domain Name** | `homelab.local` |
| **Forest Functional Level** | Windows Server 2016 |
| **Domain Functional Level** | Windows Server 2016 |
| **DNS Server** | ✅ Checked |
| **Global Catalog** | ✅ Checked |
| **DSRM Password** | `admin@123` |
| **NetBIOS Name** | HOMELAB (auto populated) |

Click through remaining screens leaving all paths as default and 
click **Install.** The server will automatically restart.

After restart log in as:
- Username: `HOMELAB\Administrator`  
- Password: `admin@123`

Your Domain Controller is now live and `homelab.local` exists.

---

## Author
**Justin Hernandez**
CIS Student — Cal Poly Pomona
IT/Cybersecurity Intern — LA-Tech.org
