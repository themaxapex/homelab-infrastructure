# Network Architecture Diagram

## Topology
    Internet (192.168.1.0/24)
             │
     Home Router (Gateway)
             │
       ┌─────────────┐
       │   vmbr0     │
       │ (WAN Bridge)│
       └──────┬──────┘
              │
       ┌─────────────┐
       │   pfSense   │
       │ WAN: 192.168.1.x
       │ LAN: 10.0.0.1
       └──────┬──────┘
              │
       ┌─────────────┐
       │   vmbr1     │
       │ (LAN Bridge)│
       └──────┬──────┘
              │
    ┌──────────────────────┐
    │ Windows Server 2022  │
    │ IP: 10.0.0.101       │
    │ GW: 10.0.0.1         │
    │ DNS: 10.0.0.1        │
    └──────────────────────┘

---

## Description

- **vmbr0 (WAN)** connects pfSense to the physical network (home router)
- **vmbr1 (LAN)** isolates internal lab network
- **pfSense** routes traffic between WAN and LAN
- **Windows Server** resides inside the internal network (10.0.0.0/24)

---

## Traffic Flow
Windows Server
↓
pfSense (LAN → WAN)
↓
Home Router
↓
Internet

---

## Security Model

- LAN (10.0.0.0/24) is isolated from home network
- All traffic must pass through pfSense firewall
- pfSense enforces routing, NAT, and DNS

---

## Key Learning

- Separation of WAN and LAN is fundamental in enterprise networks
- Virtual bridges simulate physical switches
- pfSense acts as both firewall and gateway
