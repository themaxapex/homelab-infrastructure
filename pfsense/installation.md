# 🔥 pfSense Installation on Proxmox

## 🎯 Objective

Deploy pfSense as a virtual firewall/router in Proxmox to simulate a real enterprise network with WAN and LAN segmentation.

---

## 🖥️ Environment

| Component | Details |
|----------|--------|
| Hypervisor | Proxmox VE |
| Firewall | pfSense 2.8 |
| Host | Laptop (20GB RAM) |
| Network Bridges | vmbr0 (WAN), vmbr1 (LAN) |

---

## 📥 Downloads

### Proxmox VE ISO
Downloaded from:
https://www.proxmox.com/en/downloads

---

### pfSense ISO (Netgate Installer)
Downloaded from:
https://www.pfsense.org/download/

- Architecture: AMD64
- Installer: ISO Installer
- Console: VGA

---

## ⚙️ Proxmox Setup (High-Level)

1. Installed Proxmox VE on host machine
2. Accessed web UI: https://<host-ip>:8006


3. Verified default bridge:
- vmbr0 → connected to physical network (WAN)

4. Created additional bridge for LAN:
- vmbr1 → internal network (no physical NIC)

---

## 🧱 pfSense VM Creation

### Key Configuration:

| Setting | Value |
|--------|------|
| BIOS | SeaBIOS |
| Machine Type | i440fx |
| CPU | 2 cores |
| RAM | 4GB |
| Disk | 32GB |
| Network Adapter 1 | vmbr0 (WAN) |
| Network Adapter 2 | vmbr1 (LAN) |

---

## 💿 Installation Process

1. Attached pfSense ISO to VM
2. Started VM → launched installer
3. Selected:
   - Auto (ZFS) or default filesystem
4. Confirmed disk wipe
5. Completed installation
6. Rebooted VM

---

## 🔌 Interface Assignment

During pfSense setup:

### WAN Interface:
- Assigned to: `vtnet0` → vmbr0
- Mode: DHCP
- Result:
  - Received IP from home router (192.168.1.x)

---

### LAN Interface:
- Assigned to: `vtnet1` → vmbr1

Configured manually: IP Address: 10.0.0.1
                     Subnet: 255.255.255.0 (/24)


---

## 🌐 LAN Configuration

- Enabled DHCP Server

Range: 10.0.0.100 - 10.0.0.199


---

## 🌍 Accessing pfSense GUI

From internal network: 10.0.0.1

Default login: Username: admin
               Password: pfsense


---

## ⚠️ Notes / Decisions

- Used `10.0.0.0/24` as internal network (common enterprise practice)
- WAN uses DHCP to integrate with home network
- LAN isolated using separate Proxmox bridge (vmbr1)

---

## 🧪 Verification

Tested: ping 10.0.0.1
        ping 8.8.8.8

Confirmed:
- LAN devices receive IP from pfSense
- pfSense routes traffic to internet

---

## 🧠 Key Learnings

- pfSense acts as a Layer 3 router between networks
- Network segmentation improves security
- Proxmox bridges simulate real network switches
- WAN vs LAN separation is fundamental in enterprise setups

---

## 🚀 Next Steps

- Configure Windows Server inside LAN
- Set up Active Directory
- Add domain-joined client
- Implement firewall rules

