This is a repackaging of Realtek's own 8192CU USB WiFi driver for Ubuntu 13.10 and later.

Compatibility
=============

These devices are known to work with this driver:
- Belkin N300
- TP-Link TL-WN823N
- TRENDnet TEW-648UBM N150

These devices are known not to be supported:
- Alfa AWUS036NHR
- TP-Link WN8200ND

As a rule of thumb, this driver generally works with devices that use the RTL8192CU chipset, and some devices that use the RTL8188CUS, RTL8188CE-VAU and RTL8188RU chipsets too, though it's more hit and miss.

Devices that use dual antennas are known not to work well. This appears to be an issue in the upstream Realtek driver.

Installation
============

Ensure you have the necessary prerequisites installed:

    sudo apt-get update
    sudo apt-get install git linux-headers-generic build-essential dkms

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

Other distributions
===================

The instructions above should work in every Debian-based distribution. So long as you can install the prerequisites on your own, everything after the line that contains `apt-get` should work in other distributions as well.

On Gentoo, you can disregard the instructions above and just install the following ebuild: https://gitweb.gentoo.org/dev/maksbotan.git/tree/sys-kernel/rtl8192cu-fixes/rtl8192cu-fixes-9999.ebuild

Troubleshooting
===============

There is a known issue with power management on some hardware. If your WiFi connection drops after a few minutes, install the following module setting file to disable power management in your WiFi interface:

    sudo cp ./rtl8192cu-fixes/8192cu-disable-power-management.conf /etc/modprobe.d/

And then reboot.

Current status
==============

As it currently stands, the driver doesn't populate /proc with informational data from the driver. The API for /proc has changed in recent kernels, and the driver has not been ported to the new API.

Credits
=======

This repository was initially based on Timothy Phillips's work as published here: https://code.google.com/p/realtek-8188cus-wireless-drivers-3444749-ubuntu-1304/, though no longer.

Thanks go to Saqib Razaq (@s-razaq) for the power management workaround.
