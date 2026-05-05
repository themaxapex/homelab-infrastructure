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

## ⚠️ Issue Encountered

### ❌ Problem:
No disk detected during installation

---

## 🧠 Root Cause

Windows installer does not include **VirtIO drivers**, which are required for Proxmox virtual disks.

---

## 🛠️ Solution (VirtIO Driver Injection)

### Step 1 – Download VirtIO Drivers

Downloaded from:

https://fedorapeople.org/groups/virt/virtio-win/direct-downloads/latest-virtio/

---

### Step 2 – Attach ISO to VM

Proxmox:
- Hardware → Add → CD/DVD Drive
- Selected: `virtio-win.iso`

---

### Step 3 – Load Driver in Installer

Inside Windows setup:

1. Click: Load Driver
2. Browse to: CD Drive (virtio)
              → vioscsi
              → 2k22
              → amd64
3. Install driver

---

## ✅ Result

- Disk appeared immediately
- Windows installation proceeded successfully

---

## 🔌 Network Configuration & Troubleshooting

### Initial State

After installation, the VM received an IP from the home network:
192.168.1.x

This indicated the VM was connected to the **wrong network bridge (vmbr0)** instead of the internal LAN.

---

## ⚠️ Issue 1 – Incorrect Network (Wrong Subnet)

### Problem

VM was getting IP: 192.168.1.x  Instead of: 10.0.0.x

---

### Root Cause

VM network adapter was connected to: vmbr0 (WAN) Instead of: vmbr1 (LAN behind pfSense)

---

### Solution

In Proxmox:

1. Navigate to: VM → Hardware → Network Device
2. Edit network interface: Bridge: vmbr0 → vmbr1
3. Restart VM

---

### Result

VM received correct IP: 10.0.0.101


---

## ⚠️ Issue 2 – DNS Not Working

### Problem

Internet was partially working:
ping 8.8.8.8 → success
ping google.com → failed

---

### Root Cause

pfSense firewall was not properly configured with upstream DNS servers.

---

### Solution

Logged into pfSense web interface: https://10.0.0.1

Then:

1. Navigate to: System → General Setup
2. Set DNS servers: 8.8.8.8
                    1.1.1.1
3. Disable DNS override
4. Save configuration

---

### Client Refresh

On Windows Server:

```cmd
ipconfig /flushdns
ipconfig /renew
```
Result:
ping google.com → success

🧪 Final Verification
ping 10.0.0.1
ping 8.8.8.8
ping google.com

Results:

pfSense reachable ✅
Internet reachable ✅
DNS resolution working ✅

🧠 Key Learning
Network bridge selection determines subnet assignment
pfSense acts as both gateway and DNS resolver
DNS issues can exist even when internet connectivity is functional

