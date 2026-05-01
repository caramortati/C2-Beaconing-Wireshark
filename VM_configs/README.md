# 🖥️ Virtual Lab Environment Setup (Kali Linux + Ubuntu + VirtualBox)

This lab environment was built to simulate a realistic attacker–victim network for analyzing command-and-control (C2) beaconing behavior using Wireshark. The setup consists of two virtual machines:

- **Attacker Machine:** Kali Linux  
- **Victim Machine:** Ubuntu Linux  
- **Hypervisor:** VirtualBox  
---

## ⚙️ Required Downloads

### 1. Kali Linux (C2 Server VM)
- Source: Kali Linux  
- Download: https://www.kali.org/get-kali/#kali-platforms  
- Recommended: Pre-built VirtualBox image (fastest setup)

### 2. VirtualBox (Hypervisor)
- Source: Oracle VM VirtualBox  
- Download: https://www.virtualbox.org/wiki/Linux_Downloads  

### 3. Ubuntu (Victim VM)
- Source: Ubuntu  
- Download: Ubuntou for Desktop

The virtual machines were installed using ISO images (Ubuntu and Kali Linux), which were mounted during setup and are not stored within the VM after installation.

---

## 🛠️ Environment Setup

### Step 1: Install VirtualBox
1. Download and install VirtualBox  
2. Launch the application  
3. Install VirtualBox Extension Pack for enhanced features  

---

### Step 2: Import Kali Linux VM
Guide for Installation: https://www.kali.org/docs/virtualization/install-virtualbox-guest-vm/
1. Open VirtualBox  
2. Click **File → Import Appliance**  
3. Select downloaded Kali `.ova` file  
4. Adjust settings
   - Base Memory: 4096 MB
6. Start the VM
   
<img width="681" height="892" alt="image" src="https://github.com/user-attachments/assets/36e3717c-16f6-4cae-83bd-9732d1d86fb9" />

---

### Step 3: Create Ubuntu VM
1. Click **New**  
2. Configure:
   - Name: `Ubuntu-Victim`  
   - Type: Linux  
   - Version: Ubuntu (64-bit)  
3. Allocate:
   - Base Memory: 4096 MB
   - Storage: 20GB+  
4. Mount Ubuntu ISO and complete installation
   
<img width="682" height="902" alt="image" src="https://github.com/user-attachments/assets/d6f7300b-cfe2-490c-8fb8-92040c2d60d3" />
  
---

## 🌐 Network Configuration (Critical)

To simulate attacker–victim communication:

### Option 1 (Recommended for Isolation)
- Adapter Type: **Internal Network**  
- Network Name: `lab-net`  

### Option 2 (Alternative)
- Adapter Type: **Host-Only Adapter**  

### Apply to BOTH VMs:
- Go to **Settings → Network**  
- Set **Adapter 1** to the same network  

---

## 🔍 Verification

After setup:

### On Ubuntu (Victim):
```bash
ip a
```
<img width="1283" height="801" alt="Screenshot 2026-04-26 212208" src="https://github.com/user-attachments/assets/491ffdaa-4384-4fca-9929-fbc2c6d980ca" />
