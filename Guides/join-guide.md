# How To Join A Windows Computer To A Domain

## Prerequisites
- Windows 10 Pro or Enterprise (Home edition cannot join domains)
- Access to the network the Domain Controller is on
- Domain Administrator credentials

## Steps

### 1. Verify Windows Edition
- Right click Start → System
- Confirm Edition shows Pro or Enterprise
- If Home → upgrade license before proceeding

### 2. Configure Static IP
- Right click network icon in taskbar
- Open Network and Internet Settings
- Change adapter options
- Right click adapter → Properties
- Double click Internet Protocol Version 4 (TCP/IPv4)
- Set the following:
  - IP Address: 192.168.10.x (any available address)
  - Subnet Mask: 255.255.255.0
  - Preferred DNS Server: 192.168.10.1 (Domain Controller IP)
- Click OK

### 3. Join The Domain
- Right click start -> System
- Click Rename this PC (Advanced)
- Rename this PC to something descriptive (e.g. CLIENT01, CLIENT02)
- Click the Change button
- Select Domain radio button
- Enter domain name: homelab.local
- Click OK
- Enter Administrator credentials when prompted
- Click OK on Welcome message

### 4. Restart
- Click OK → Restart Now
- Wait for machine to fully reboot

### 5. Login With Domain Account
- On login screen click Other User
- Enter domain credentials (e.g. john.smith)
- Change password if prompted on first login

## Verification
Open Command Prompt and run:

```cmd
whoami
```
Should return: `homelab\username`

```cmd
echo %logonserver%
```
Should return your Domain Controller name (e.g. `\\WIN-1FGEG1FMJLD`)
