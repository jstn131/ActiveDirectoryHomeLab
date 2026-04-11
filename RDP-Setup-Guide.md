# RDP Setup Guide — Remote Desktop Protocol

## What Is RDP?

RDP (Remote Desktop Protocol) operates on **TCP port 3389**. As the 
name suggests, RDP allows you to remotely control a desktop from a 
centralized device without physically being at that machine.

This is extremely useful for helpdesk technicians — instead of walking 
to every device in an organization to fix an issue, a technician can 
remote in from their own workstation and resolve the problem instantly.

RDP is primarily a Windows protocol but alternatives exist for other 
operating systems such as VNC for Linux and Apple Remote Desktop for macOS.
RDP can provide either full desktop control or application-only control 
depending on how it is configured.

### Security Considerations
Because RDP provides full desktop access it is one of the most frequently 
targeted protocols by attackers. Port 3389 exposed to the internet receives 
thousands of automated login attempts daily. For this reason most organizations:
- Restrict RDP to internal networks only
- Require VPN before RDP is accessible
- Use Network Level Authentication (NLA) for added security
- Monitor port 3389 for unauthorized access attempts

---

## Prerequisites
- Windows 10 Pro or Enterprise on the target machine
  (RDP hosting is not available on Windows Home)
- Administrator credentials for the target machine
- Both machines on the same network

---

## Step 1 — Enable RDP On The Target Machine

Since this is our first time setting up RDP in our domain we must 
physically access the device we want to control — in our case CLIENT01.

Right click **Start → System** and scroll down to **Remote Desktop**

*Screenshot showing System About page with Remote Desktop option*
<img width="482" height="625" alt="Screenshot 2026-04-11 152725" src="https://github.com/user-attachments/assets/ce66d17b-2f25-4ad4-aa71-e20da39c4142" />

Click Remote Desktop and toggle **Enable Remote Desktop** to **On.**

> **Important:** You must be logged in as an Administrator to enable 
> this feature. Standard domain users will see this instead:

*Screenshot showing restricted access message for non-admin users*
<img width="482" height="623" alt="Screenshot 2026-04-11 152749" src="https://github.com/user-attachments/assets/2a21b4fe-8a79-48f5-bb17-9399c0754b30" />

---

## Why Must I Log In As Admin To Enable RDP?
## Doesn't That Defeat The Purpose?

**Short answer — yes, in a home lab it does feel contradictory.**

In a real enterprise environment this problem is solved entirely by 
**Group Policy.** An IT administrator creates a single Group Policy 
Object (GPO) on the Domain Controller that enables RDP across every 
domain joined machine simultaneously. One change on the server pushes 
to every machine automatically — no physical access to individual 
machines ever needed.

The manual login we perform in this lab is purely because Group Policy 
has not been configured yet. It is a one time lab workaround, not 
how production environments operate.

Enterprise RDP Setup Flow:
IT Admin creates one GPO on Domain Controller
↓
GPO pushes to every domain joined computer automatically
↓
RDP enabled on all machines simultaneously
↓
Helpdesk can remote into any machine instantly
↓
No physical access to individual machines ever needed

---

## Step 2 — Note The Computer Name

Once RDP is enabled you will see the computer's connection name at 
the bottom of the Remote Desktop settings page. This is important 
as you will need it to connect remotely.

*Screenshot showing successful RDP enable and PC connection name*
<img width="470" height="629" alt="Screenshot 2026-04-11 153619" src="https://github.com/user-attachments/assets/f15b8260-baa9-4cfa-a94e-ee95ee246da6" />

In our lab the connection name is: CLIENT01.homelab.local

You can connect using either this name or the machine's IP address `192.168.10.2`

---

## Step 3 — Connect From The Domain Controller

On your Domain Controller (or any administrator machine) search for 
**Remote Desktop Connection** in the Start menu.

*Screenshot showing Remote Desktop Connection in Start menu search*
<img width="1017" height="771" alt="Screenshot 2026-04-11 153759" src="https://github.com/user-attachments/assets/4afff16a-b681-44d7-b273-3ae36bc9e8e5" />

Enter the computer name or IP address of the machine you want to 
control and click **Connect.**

Enter your Administrator credentials when prompted.

---

## Step 4 — Verify Successful Connection

Once connected you will see the target machine's desktop inside an 
RDP window. The blue bar at the top shows the name of the machine 
you are remotely controlling — confirming the connection was successful.

*Screenshot showing successful RDP session with CLIENT01.homelab.local in title bar*
<img width="1024" height="770" alt="Screenshot 2026-04-11 153934" src="https://github.com/user-attachments/assets/c0a747e8-9b36-4ba5-8583-ecb0d67c79db" />

---

## Session Behavior — Important To Understand

When you RDP into a machine using the same account that is currently 
logged in locally, the local session will be disconnected and taken 
over by the RDP session. This is expected behavior.

> **Helpdesk Etiquette:** Always notify the user before remoting into 
> their machine. Connecting without warning will immediately disconnect 
> their active session which can cause them to lose unsaved work.

*Screenshots showing local session disconnecting when RDP connects*
<img width="2646" height="1259" alt="Screenshot 2026-04-11 154219" src="https://github.com/user-attachments/assets/76012eee-94cb-451b-9f98-374c0a50bc43" />

<img width="2619" height="1431" alt="Screenshot 2026-04-11 154235" src="https://github.com/user-attachments/assets/893375df-3dcc-476e-8be1-b9f73bc728f4" />

---

## Key Takeaways

- RDP operates on **TCP port 3389**
- Only **Windows Pro and Enterprise** can host RDP sessions
- Always **notify users** before remoting into their machine
- In production environments **Group Policy** enables RDP across 
  all machines automatically — no manual setup per device
- RDP is a common attack target — always secure it with VPN 
  and Network Level Authentication in production environments

---
