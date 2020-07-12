# 1. NVIDIA Drivers
Although its possible to download [NVIDIA drivers for freeBSD](https://www.nvidia.com/Download/driverResults.aspx/162108/en-us) the easiest would be to install it from [freshports](https://www.freshports.org/x11/nvidia-driver/) with
```
pkg install -y nvidia-driver
```
Then [setup NVIDIA.](https://forums.freebsd.org/threads/howto-setup-xorg-with-nvidias-driver.52311/)
Note that [these instructions are outdated.](https://docs.freebsd.org/doc/7.3-RELEASE/usr/share/doc/en/articles/compiz-fusion/nvidia-setup.html)
## 1.1. Load the kernel module created by the driver at boot time.
This is done by adding below to `vi /etc/rc.conf`
```
kld_list="nvidia-modeset"
```
## 1.2. Enable the driver.
Create the `/xorg.conf.d` directory as follows
```
mkdir -p /usr/local/etc/X11/xorg.conf.d
```
then, add below into `vi /usr/local/etc/X11/xorg.conf.d/driver-nvidia.conf`
```
Section "Device"
        Identifier "NVIDIA Card"
        VendorName "NVIDIA Corporation"
        Driver "nvidia"
EndSection
```

# 2. Optional tools/utilities for NVIDIA
Although not required it is handy to install some NVIDIA tools.
## 2.1. Display Control Panel for X NVidia driver
```
pkg install -y nvidia-settings
```
## 2.2. Tool to manipulate X configuration files for the NVidia driver
```
pkg install -y nvidia-xconfig
```
[Read these comments](https://forums.freebsd.org/threads/howto-install-and-configure-nvidia-drivers.3038/) for discussions on installing NVIDIA drivers in FreeBSD.

# 3. Install Linux programs and libraries
Because `x11/nvidia-driver` port no longer installs Linux programs and libraries if you plan to use linuxulator you will want to install these libraries.
But before installing these you must [configure linux binary compatibility](https://www.freebsd.org/doc/handbook/linuxemu-lbc-install.html).

## 3.1. Load Linux kernel module
```
kldload linux
kldload linux64
```
You can use `kldstat` to verify.
## 3.2. Install the NVIDIA linux programs
```
pkg install -y linux-nvidia-libs
```
