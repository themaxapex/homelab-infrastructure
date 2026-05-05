# 🖥️ Proxmox VE Installation & Lab Setup

## 🎯 Objective

Prepare the hypervisor environment and deploy virtual machines to simulate an enterprise network.

---

## 📥 Downloads

### Proxmox VE ISO
Downloaded from:
https://www.proxmox.com/en/downloads

---

### Windows Server ISO
Downloaded from Microsoft Evaluation Center:
https://www.microsoft.com/en-us/evalcenter/

---

### pfSense ISO
Downloaded from:
https://www.pfsense.org/download/

---

## 💿 Creating Bootable USB

Used **Rufus** to create bootable Proxmox installer:

Steps:
1. Insert USB
2. Open Rufus
3. Select Proxmox ISO
4. Partition scheme: GPT
5. Target system: UEFI
6. Start

---

## ⚙️ Installing Proxmox

1. Booted from USB
2. Selected "Install Proxmox VE"
3. Configured:
   - Disk (default)
   - Timezone
   - Password
   - Network (DHCP)

---

## 🌐 Accessing Proxmox

After installation: https://<proxmox-ip>:8006

Logged in with:
- User: root
- Password: (configured during install)

---

## 📤 Uploading ISOs

From Proxmox GUI:

1. Datacenter → Storage → local
2. Click "ISO Images"
3. Upload:
   - pfSense ISO
   - Windows Server ISO

---

## 🔌 Network Setup

Configured:

- vmbr0 → WAN (connected to physical network)
- vmbr1 → LAN (internal network)

---

## 🧱 Virtual Machines Created

- pfSense VM (Firewall)
- Windows Server 2022 VM

---

## 🧠 Key Learnings

- Hypervisors manage virtual infrastructure
- Proxmox bridges simulate real network switches
- Proper ISO preparation is critical before deployment
