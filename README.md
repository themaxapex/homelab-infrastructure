# Enterprise Homelab – Proxmox + pfSense + Windows Server

## Objective
Build a virtual enterprise network using Proxmox, pfSense, and Windows Server to simulate real-world infrastructure. 
We documented the full network topology including WAN/LAN segmentation and traffic flow through pfSense.

---

## Architecture

Internet (192.168.1.x)
        ↓
   pfSense (WAN)
        ↓
   pfSense (LAN: 10.0.0.1)
        ↓
 Windows Server (10.0.0.101)

---

## Technologies

- Proxmox VE
- pfSense Firewall
- Windows Server 2022
- VirtIO Drivers

---

## Key Features

- Network segmentation (WAN/LAN)
- Firewall and routing via pfSense
- Internal LAN (10.0.0.0/24)
- Windows Server deployment
- DNS + connectivity troubleshooting
- Virtual disk driver resolution (VirtIO)

---

## Key Learnings

- How enterprise networks are structured
- Troubleshooting missing disk drivers
- Configuring DNS and gateways
- Working with virtual networking in Proxmox

---

## Project Structure

- proxmox → Hypervisor setup
- pfsense → Firewall + network config
- windows-server → OS + drivers
- diagrams → Network design

---

## Next Steps

- Active Directory Domain Controller
- Domain-joined client VM
- Group Policy (GPO)
- VPN setup (pfSense)
- Azure hybrid lab

---
## Network Diagram

See full diagram:
[View Network Architecture](diagrams/network.md)

---

## Author

Max Yousefi
