# Realtek RTL8126-CG 5Gb LAN driver on Debian-12/Proxmox
Realtek RTL8126-CG is not supported by the default Linux kernel in Debian 12 or Proxmox out-of-the-box. It requires manual driver installation.

> This repository provides a step-by-step guide to install and configure the RTL8126-CG 5GbE LAN driver, including prerequisites, verified sources, DKMS instructions, and troubleshooting tips for Proxmox VE and Debian systems.

We'll assume you're using a fresh Proxmox or Debian 12 install, and you need to manually build and install the driver from Realtek's official source.
## 🛠 Step-by-Step Installation Instructions

### ✅ Step 1: Install Required Packages
These packages are needed to build kernel modules.

```
sudo apt update
sudo apt install -y build-essential dkms git linux-headers-$(uname -r)
```
### ✅ Step 2: Download Realtek Driver Source
Go to the official Realtek site and download the latest r8125 driver (which covers RTL8126-CG as well):
> 1. Visit: `https://www.realtek.com/en/component/zoo/category/network-interface-controllers-10-100-1000m-gigabit-ethernet-pci-express-software`
> 2. Look for something like: `Linux driver for kernel up to 5.x for RTL8125 / RTL8126 / RTL8111 family`
> 3. Download the `.tar.bz2 archive` and copy it to your Debian/Proxmox system.
### ✅ Step 3: Extract and Enter the Directory
```
tar -xf r8125-*.tar.bz2
cd r8125-*/
```
### ✅ Step 4: Run the Installer Script
This script builds and installs the kernel module:
```
sudo ./autorun.sh
```
It may take a minute or two.
### ✅ Step 5: Reboot the System
```
sudo reboot
```
### ✅ Step 6: Verify the Interface is Active
Check network devices:
```
ip link show
```
Check the hardware and driver info:
```
sudo lshw -C network
```
Or:
```
dmesg | grep r8125
```
## 🧠 Important Note

> ⚠️ If your system only has a single Realtek RTL8126-CG NIC and no other working network interface, you’ll need **an additional NIC** (e.g., USB Ethernet or Wi-Fi adapter) to proceed with the driver installation steps, since the RTL8126-CG won’t be usable until after setup.  
>
> 📘 A detailed offline installation guide and alternative setup method (e.g., preloading driver via USB or using temporary networking) will be added soon.
