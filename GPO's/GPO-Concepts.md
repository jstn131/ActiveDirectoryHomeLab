# Group Policy Objects — Concepts and Reference Guide

## What Is Group Policy?

Group Policy is a feature of Active Directory that allows IT 
administrators to push configurations and settings to specific 
groups of users and computers from one centralized location. 
Instead of configuring each machine individually, an admin 
creates one policy and it automatically applies to everyone 
in the targeted Organizational Unit. Kind of like how an MDM
pushes updates to devices. 

---

## Computer Configuration vs User Configuration

GPOs have two types of configurations:

**Computer Configuration** — settings that affect the machine 
itself regardless of who logs in. If a Computer Configuration 
policy is applied to a workstation, those settings stick no 
matter which user sits down and logs in.

**User Configuration** — settings that follow the user 
regardless of which machine they log into. If john.smith 
logs into Computer A, Computer B, or Computer C, his User 
Configuration policies follow him everywhere.

| | Computer Configuration | User Configuration |
|---|---|---|
| **Follows** | The machine | The user |
| **Applies when** | Computer starts up | User logs in |
| **Example** | Screen lock timer | Desktop wallpaper |

---

## Policies vs Preferences

**Policies** are security standards that users and computers 
must follow. They are enforced by the domain and cannot be 
overridden by the user. For example disabling Control Panel 
is a Policy — john.smith cannot re-enable it no matter what.

**Preferences** are convenience settings that make things 
easier for users but are not strictly enforced. Users can 
technically change them. Drive mapping is a Preference — 
it automatically connects a shared drive for convenience 
but is not a security requirement.

| | Policies | Preferences |
|---|---|---|
| **Enforced?** | Yes — user cannot override | No — user can change |
| **Use for** | Security requirements | Convenience settings |
| **Example** | Disable Control Panel | Map network drive |

---

## GPO Inheritance

GPO Inheritance works similarly to inheritance in Object 
Oriented Programming. Think of it like a tree data structure 
where the root node passes properties down to all children:

```
Finance OU (Root)
└── GPO linked here (Parent)
      ↓ inherits automatically
      ├── Finance-Users sub OU (Child) ✅
      └── Finance-Computers sub OU (Child) ✅
```

Any GPO linked to a parent OU automatically flows down 
to all child sub OUs underneath it. This means you only 
need to link a GPO once at the parent OU level and it 
applies to everything below it.

Inheritance can be blocked on specific child OUs if needed 
but by default everything flows downward automatically.

---

## Naming Conventions

GPO names should immediately tell any technician what the 
policy does and who it applies to:

```
Format: [Department]-[What It Does]-Policy

Examples:
Finance-Security-Policy
Finance-Desktop-Policy
Finance-Restrictions-Policy
Finance-DriveMaps-Policy
```

Good naming conventions prevent duplicate GPOs from being 
created and make auditing and troubleshooting faster.

---

## GPOs Created In This Lab

### Finance-Security-Policy
**Type:** Computer Configuration  
**Location:** Security Settings → Local Policies → Security Options  
**Setting:** Machine Inactivity Limit = 300 seconds (5 minutes)

If a Finance department computer sits idle for 5 minutes 
the screen automatically locks and prompts the user to 
log back in. This applies to the machine regardless of 
who is logged in.

---

### Finance-Desktop-Policy
**Type:** User Configuration  
**Location:** Administrative Templates → Desktop → Desktop  
**Setting:** Desktop Wallpaper = C:\Windows\Web\Wallpaper\Theme2\img7.jpg

Forces a specific wallpaper on all Finance department 
user accounts. Follows the user to any computer they 
log into.

> **Troubleshooting Note:** An incorrect file path results 
> in a black desktop. Always verify the exact file path 
> exists on the target machine before deploying.

---

### Finance-Restrictions-Policy
**Type:** User Configuration  
**Location:** Administrative Templates → Control Panel  
**Setting:** Prohibit access to Control Panel and PC Settings = Enabled

Prevents Finance department users from opening Control 
Panel or Windows Settings. Control Panel shows a 
restriction error. Settings closes immediately on launch.

---

### Finance-DriveMaps-Policy
**Type:** User Configuration → Preferences  
**Location:** Preferences → Windows Settings → Drive Maps  
**Settings:**

```
Action:      Update
Location:    \\WIN-1FGEG1FMJLD\Finance-Folder
Drive Letter: F:
Label:       Finance Drive
Reconnect:   Enabled
```

Automatically maps the Finance shared folder as F: drive 
for all Finance users on login. Appears automatically in 
File Explorer without any manual configuration by the user.

---

## Testing GPOs

### Force Immediate Policy Application

```cmd
gpupdate /force
```

Forces the computer to immediately pull all new or updated 
Group Policies from the Domain Controller instead of waiting 
for the automatic 90 minute refresh cycle.

### Verify Which Policies Are Applied

```cmd
gpresult /r
```

Run as Administrator to see all Group Policies currently 
applied. Look for your GPO names under Applied Group 
Policy Objects.

### When To Log Out And Back In

User Configuration policies apply at login. After running 
gpupdate /force always log out and log back in to ensure 
User Configuration settings apply properly.

---

## Complete Finance OU GPO Summary

| GPO | Type | What It Does |
|---|---|---|
| Finance-Security-Policy | Computer Config | Screen locks after 5 minutes idle |
| Finance-Desktop-Policy | User Config | Forces company wallpaper |
| Finance-Restrictions-Policy | User Config | Disables Control Panel and Settings |
| Finance-DriveMaps-Policy | User Config Preference | Auto maps F: Finance shared drive |

---

## Key Takeaways

- Group Policy pushes configurations to entire departments 
  from one central location
- Computer Configuration follows the machine
- User Configuration follows the user
- Policies are enforced — Preferences are convenience settings
- GPOs inherit downward from parent OUs to all child sub OUs
- Always use clear naming conventions
- `gpupdate /force` pushes policies immediately
- `gpresult /r` confirms which policies are applying
