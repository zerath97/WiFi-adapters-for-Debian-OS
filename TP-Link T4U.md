# RTL88x2bu Driver Installation for TP-Link Archer T4U on Debian

This document outlines the steps required to successfully install the RTL88x2bu driver for the TP-Link Archer T4U wireless adapter on a Debian-based system.

## Requirements
- A TP-Link Archer T4U wireless adapter (AC1300)
- A Debian-based system (tested on Debian 11)
- Kernel headers installed for your specific kernel version

## Steps

### Step 1: Clone the RTL88x2bu Driver Repository
First, clone the repository that contains the RTL88x2bu driver source code.

```bash
git clone https://github.com/aircrack-ng/rtl88x2bu.git
cd rtl88x2bu
```
### Step 2: Install Required Dependencies
Ensure that you have the necessary tools and libraries to build the driver.

```bash
sudo apt update
sudo apt install dkms build-essential libelf-dev linux-headers-$(uname -r) git
```
### Step 3: Configure DKMS
To automate the building and installation of the driver for future kernel updates, configure the driver as a DKMS module.

1. Copy the driver source code to the `/usr/src` directory:

   ```bash
   sudo cp -r ~/rtl88x2bu /usr/src/rtl88x2bu-1.0
   ```
2. Add the module to DKMS:
   ```bash
   sudo dkms add -m rtl88x2bu -v 1.0
   ```

### Step 4: Modify the `dkms.conf` File
Edit the `dkms.conf` file to ensure proper building of the driver.

```bash
sudo nano /usr/src/rtl88x2bu-1.0/dkms.conf
```

Find the MAKE[0] line and change it to:
```bash
MAKE="'make' -j $PROCS_NUM KVER=$kernelver"
```
Save and exit (Ctrl+O, Enter, Ctrl+X).

### Step 5: Build the Driver
Now, build the driver using DKMS.

```bash
sudo dkms build -m rtl88x2bu -v 1.0
```

### Step 6: Install the Driver
Once the build is successful, install the driver module:

```bash
sudo dkms install -m rtl88x2bu -v 1.0
```

### Step 7: Load the Driver Module
Load the driver into the kernel:

```bash
sudo modprobe 88x2bu
```

### Step 8: Verify the Installation
Check that the driver is loaded and the wireless interface is available:

1. Verify the driver is loaded:

```bash
lsmod | grep 88x2bu
```

2. Check the network interfaces:

```bash
ip link show
```

### Step 9: Connect to a Network
You should now be able to scan and connect to available Wi-Fi networks. Use a tool like nmcli or a graphical network manager.

#### Terminal
```bash
nmcli dev wifi list
```

#### WiFi GUI
1. If using the WiFi network GUI, a new tab should appear at the top of this window: "TP-Link Archer T4U...".
2. Connect to the WiFi you normally would use, however within this new tab.

That's it!

