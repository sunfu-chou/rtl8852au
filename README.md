# RTL8852AU Linux Driver on Ubuntu 20.04 for DWA-X1850

### This repo is a fork from [lwfinger/rtl8852au](https://github.com/lwfinger/rtl8852au)

RTL8852AU driver is tested on Ubuntu 20.04 and Using DWA-X1850.
The driver supports rtl8832au/rtl8852au chipsets.

Here is the tutorial for install driver.

#### 0. Install dependencies
* BUFFALO WI-U3-1200AX2(/N) with USB ID 0411:0312
* ASUS USB-AX56 with USB ID 0b05:1997
* ASUS USB-AX56 with USB ID 0b05:1a62
* EDUP EP-AX1696GS with USB ID 0bda:8832
* Fenvi FU-AX1800P with USB ID 0bda:885c
* Realtek Demo Board with USB ID 0bda:8832
* Realtek Demo Board with USB ID 0bda:885a
* Realtek Demo Board with USB ID 0bda:885c
* D-Link DWA-X1850 with USB ID 2001:3321
* TP-Link AX1800 with USB ID 2357:013f
* ipTIME AX2000U with USB ID 0bda:8832

```
sudo apt-get update
sudo apt-get install make gcc linux-headers-$(uname -r) build-essential git
```
#### 1. Clone this repo

```
git clone git@github.com:sunfu-chou/rtl8852au.git
For **openSUSE**: Install necessary headers with
```bash
sudo zypper install make gcc kernel-devel kernel-default-devel git libopenssl-devel
```
For **Arch**: After installing the necessary kernel headers and base-devel,
```bash
git clone https://aur.archlinux.org/rtw89-dkms-git.git
cd rtw89-dkms-git
makepkg -sri
```
If any of the packages above are not found check if your distro installs them like that.

##### Installation
When a USB device is plugged in, or detected at boot, this rule causes the utulity
usb_modeswitch to unload any 0bda:1a2b devices that it finds. If you have a
device with different ID, change the rule accordingly.

The build this driver, do the following:

For all distros:
```bash
git clone https://github.com/lwfinger/rtl8852au.git
cd rtl8852au
make
sudo make install

When you get a new kernel, you will need to rebuild the driver. Do the following:
cd rtl8852au
git pull
make
sudo make install
```

#### 2. Use DKMS to build and install

```
cd rtl8852au

# Add module to dkms tree
sudo dkms add .

# Build 
sudo dkms build rtl8852au -v 1.15.0.1

# Install 
sudo dkms install rtl8852au -v 1.15.0.1

# Check installation
sudo modinfo 8852au

# Load driver 
sudo modprobe 8852au
```

### 3. Switch USB mode to USB 3.0

Add this line below into `/etc/modprobe.d/8852au.conf`

```
options 8852au rtw_switch_usb_mode=1
```

### 4. Unmount disk

Add this line below into `/lib/udev/rules.d/40-usb_modeswitch.rules`

```
ATTR{idVendor}=="0bda", ATTR{idProduct}=="1a2b", RUN+="/usr/sbin/usb_modeswitch -K -v 0bda -p 1a2b"
```

### If you still meet 0bda problem on D-link DWA-X1850
go to Files > right click to the usb device > choose eject
```
lsusb -t 
#you can see the usb device status

```

