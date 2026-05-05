# 🖥️ Windows Server 2022 Installation on Proxmox

## 🎯 Objective

Deploy Windows Server 2022 as an internal server within the pfSense LAN (10.0.0.0/24) and resolve virtualization-related driver issues.

---

## 📥 Download

Windows Server ISO downloaded from:

https://www.microsoft.com/en-us/evalcenter/

---

## 🧱 VM Configuration (Proxmox)

| Setting | Value |
|--------|------|
| OS | Windows Server 2022 |
| BIOS | SeaBIOS |
| Machine | i440fx |
| CPU | 2 cores |
| RAM | 8GB |
| Disk | 32GB |
| Disk Type | SCSI (VirtIO) |
| Network | vmbr1 (LAN) |

---

## 💿 Installation Process

1. Created VM in Proxmox
2. Attached Windows Server ISO
3. Started VM and launched installer
4. Selected:
   - Windows Server 2022 Standard (Desktop Experience)

---

# 🏢 Active Directory Domain Controller Setup

## 🎯 Objective

Configure Windows Server 2022 as a Domain Controller with Active Directory Domain Services (AD DS) and DNS in a virtual enterprise lab environment.

---

## 🧱 Prerequisites

Before installing AD DS, the following steps were completed:

### 1. Rename Server

Renamed system to:

DC-TMA

Reason:
Renaming before domain promotion avoids complexity and prevents issues after the server becomes a Domain Controller.

---

### 2. Configure Static IP Address

Changed network configuration from DHCP to static:

IP Address: 10.0.0.101  
Subnet Mask: 255.255.255.0  
Default Gateway: 10.0.0.1  
DNS: 10.0.0.1 (pfSense)

Reason:
Domain Controllers require a static IP to maintain consistent network identity and reliable DNS resolution.

---

## Installing Active Directory Domain Services

Steps:

1. Open Server Manager
2. Navigate to:
   Manage → Add Roles and Features
3. Select:
   Role-based or feature-based installation
4. Choose the local server
5. Enable:
   - Active Directory Domain Services
   - DNS Server
6. Proceed with installation

---

## Promoting to Domain Controller

After installation:

1. Click:
   Promote this server to a domain controller
2. Select:
   Add a new forest
3. Enter domain name:

TMA.local

4. Set Directory Services Restore Mode (DSRM) password
5. Ignore DNS delegation warning (expected in new forest)
6. Proceed with installation
7. Server reboots automatically

---

## Post-Installation Configuration

### 1. Verify Domain

Open Command Prompt:

echo %USERDOMAIN%

Expected result:
TMA

---

### 2. Update DNS Settings

After promotion, change DNS from pfSense to local server:

Preferred DNS: 127.0.0.1  
(or 10.0.0.101)

Reason:
Domain Controllers must use themselves as DNS servers for proper Active Directory functionality.

---

### 3. Test Name Resolution

Run:

nslookup google.com

---

## 🧪 Validation

- Domain successfully created (TMA.local)
- DNS functioning correctly
- Server operating as Domain Controller (DC-TMA)

---

## Key Learnings

- Domain Controllers must have a static IP
- AD DS relies heavily on DNS
- Correct configuration sequence prevents deployment issues
- External DNS should not be used after domain promotion

---

## Next Steps

- Create Organizational Units (OU)
- Add users and groups
- Configure Group Policy (GPO)
- Join client VM to domain
