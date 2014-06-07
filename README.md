This is a port of Realtek's own 8192cu driver for USB Wifi chipsets, ported to kernel 3.11+ as ships with Ubuntu 13.10 and later.

It was initially based on Timothy Phillips's work as published here: https://code.google.com/p/realtek-8188cus-wireless-drivers-3444749-ubuntu-1304/, though no longer.

Installation
============

Ensure you have the necessary prerequisites:

    sudo apt-get install linux-headers-generic build-essential dkms

Clone this repository:

    git clone https://github.com/pvaret/rtl8192cu-fixes.git

Set it up as a DKMS module:

    sudo dkms add ./rtl8192cu-fixes

Build and install it:

    sudo dkms install 8192cu/1.9

Refresh the module list:

    sudo depmod -a

Ensure the native (and broken) kernel driver is blacklisted:

    sudo cp ./rtl8192cu-fixes/blacklist-native-rtl8192.conf /etc/modprobe.d/

And reboot. You're done.

Troubleshooting
===============

There is a known issue with power management on some hardware. If your WiFi connection drops after a few minutes, install the following module setting file to disable power management in your WiFi interface:

    sudo cp ./rtl8192cu-fixes/8192cu-disable-power-management.conf /etc/modprobe.d/

And then reboot. (Credit goes to Saqib Razaq for the fix.)

Current status
==============

As it currently stands, the driver doesn't populate /proc with informational data from the driver. The API for /proc has changed, and the driver hasn't yet been ported to the new API.

This driver is known to work with RTL8192cu devices like the Belkin N150, but may not work with hardware based on other chipsets like the 8188cus, nor with devices using a 2x2 setup like the TP-Link WN8200ND.
