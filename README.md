This is a port of Realtek's own 8192CU driver for USB WiFi devices on Ubuntu 13.10 and later.

Installation
============

Ensure you have the necessary prerequisites installed:

    sudo apt-get install linux-headers-generic build-essential dkms

Clone this repository:

    git clone https://github.com/pvaret/rtl8192cu-fixes.git

Set it up as a DKMS module:

    sudo dkms add ./rtl8192cu-fixes

Build and install it:

    sudo dkms install 8192cu/1.10

Refresh the module list:

    sudo depmod -a

Ensure the native (and broken) kernel driver is blacklisted:

    sudo cp ./rtl8192cu-fixes/blacklist-native-rtl8192.conf /etc/modprobe.d/

And reboot. You're done.

Troubleshooting
===============

There is a known issue with power management on some hardware. If your WiFi connection drops after a few minutes, install the following module setting file to disable power management in your WiFi interface:

    sudo cp ./rtl8192cu-fixes/8192cu-disable-power-management.conf /etc/modprobe.d/

And then reboot.

Current status
==============

As it currently stands, the driver doesn't populate /proc with informational data from the driver. The API for /proc has changed in recent kernels, and the driver hasn't yet been ported to the new API.

This driver is known to work with some RTL8192CU devices like the Belkin N300, and should work with some 8188CUS, RTL8188CE-VAU and RTL8188RU devices as well. However, it is known not to work well with devices using dual antennas, such as the TP-Link WN8200ND, due to an incomplete MIMO implementation in the upstream driver.

Credits
=======

This repository was initially based on Timothy Phillips's work as published here: https://code.google.com/p/realtek-8188cus-wireless-drivers-3444749-ubuntu-1304/, though no longer.

Thanks go to Saqib Razaq (@s-razaq) for the power management workaround.
