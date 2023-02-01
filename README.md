# RTL8852AU Linux Driver on Ubuntu 20.04 for DWA-X1850

### This repo is a fork from [lwfinger/rtl8852au](https://github.com/lwfinger/rtl8852au)

RTL8852AU driver is tested on Ubuntu 20.04 and Using DWA-X1850.

Here is the tutorial for install driver.

#### 0. Install dependencies

```
sudo apt-get update
sudo apt-get install make gcc linux-headers-$(uname -r) build-essential git
```
#### 1. Clone this repo

```
git clone git@github.com:sunfu-chou/rtl8852au.git
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

Set the velue in `/sys/module/8852au/parameters/rtw_switch_usb_mode` is `1`

```
sudo sh -c "echo '1' > /sys/module/8852au/parameters/rtw_switch_usb_mode"
```

Add this line below into `/etc/modules-load.d/8852au.conf`

```
options 8852au rtw_switch_usb_mode=1
```

Add this line below into `/lib/udev/rules.d/40-usb_modeswitch.rules`

```
ATTR{idVendor}=="0bda", ATTR{idProduct}=="1a2b", RUN+="usb_modeswitch '/%k'"
```
