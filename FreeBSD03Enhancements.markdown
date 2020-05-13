# Enhancements

By enabling a locale using UTF-8 character encoding,
the system can understand and display each of the 1112064 characters in the Unicode character set,
instead of just US ASCII as its default with LANG=C.

1. Some third-party software options can only be set at compile time.
  You should consider the following softwares:
  - Common UNIX Printing System (CUPS),
  - Network Audio System (NAS) and
  - Qt style that renders using GTK (QGTKSTYLE).
  
  Do this with
```
echo "QT4_OPTIONS= CUPS QGTKSTYLE NAS" >> /etc/make.conf
```
  Note that if you change `QT4_OPTIONS` **after** Qt toolkit is installed
  *you will need to rebuild* `devel/qt4-corelib` and `x11-toolkits/qt4-gui`.

2. Changes to `vi /etc/sysctl.conf` file include
  - expanding amount of shared memory,
  - tuning process scheduler and
  - increasing the limit of simultaneously open files (to something sensible).

```
# Enhance shared memory X11 interface
kern.ipc.shmmax=67108864
kern.ipc.shmall=32768
# Enhance desktop responsiveness under high CPU use (200/224)
kern.sched.preempt_thresh=224
# Bump up maximum number of open files
kern.maxfiles=200000
# Disable PC Speaker
hw.syscons.bell=0
# Shared memory for Chromium
kern.ipc.shm_allow_removed=1
```
  Note that:
  - `printf "\a"` to test the beep sound or its absence
  - `kern.ipc.shm_allow_removed=1` removes the need of manually changing this flag for Chrome users.
    * It also improves compatibility with Linux applications running under Linuxulator compatibility layer,
    and possibly also helps in porting software from Linux.
    * The flag allows applications to create the shared memory segment, attach it, remove it,
      and then continue to use it and to reattach it later.
      - This means that the kernel will automatically "clean up" after the application exits.

3. Some enhancements can only be set at boot in `vi /boot/loader.conf`.
  This is also where you define kernel modules to load at boot. [p662 Absolute FreeBSD 2ndEd]
  See this [example `/boot/loader.conf`.](https://blog.wedrop.it/freebsd/2016/04/23/freebsd-setup.html)
```
# Use new graphical console driver
kern.vty=vt
# Devil worship in loader logo
loader_logo="beastie"
# Boot-time kernel tuning [https://en.wikipedia.org/wiki/Shared_memory]
kern.ipc.shmseg=1024  # maximum number of System V shared memory segments any one process can open
kern.ipc.shmmni=1024  # maximum number of System V shared memory segment identifiers in the system
kern.maxproc=10000    # maximum number of processes the system can run at any one time
# Load MMC/SD card-reader support
mmc_load="YES"
mmcsd_load="YES"
sdhci_load="YES"
# Access ATAPI devices through the CAM subsystem
atapicam_load="YES" # Even if the computer has modern sata drives and compatible with SAS controllers
# Filesystems in Userspace
fuse_load="YES"     # to mount NTFS drives under FreeBSD
# CPU thermal sensors
coretemp_load="YES" # Intel Core thermal sensors
#amdtemp_load="YES" # AMD K8, K10, K11 thermal sensors
# In-memory filesystems
tmpfs_load="YES"
# Asynchronous I/O
aio_load="YES"
# Handle Unicode on removable media
libiconv_load="YES"
libmchain_load="YES"
cd9660_iconv_load="YES"
msdosfs_iconv_load="YES"
# Load sound subsystem
snd_driver_load="YES"
# Enable better trackpad support
hw.psm.synaptics_support=1
[See https://blog.wedrop.it/freebsd/2016/04/23/freebsd-setup.html]
```
  - `kern.vty=vt`
    * [Test intel driver by typing `kldload i915kms`](https://wiki.freebsd.org/Newcons https://www.freebsd.org/cgi/man.cgi?query=vt&sektion=4#end)
  - `loader_logo="beastie"`
    * [More on logo](http://www.freebsdmadeeasy.com/tutorials/freebsd/beastie-boot-ascii.php)
  - `mmc_load="YES"`
    * [More on MMC/SD](https://www.freebsd.org/cgi/man.cgi?sdhci(4))
  - `atapicam_load="YES"`
    * [See this](http://www.ngtech.co.il/fbsd_old/handbook/creating-cds.html) and [this.](http://www.root.org/~nate/freebsd/scsi/quirks.html)
  - `coretemp_load="YES"`
    * [On Intel core thermal sensors](https://www.freebsd.org/cgi/man.cgi?coretemp)
  - `amdtemp_load="YES"`
    * [On AMD thermal sensors](https://www.freebsd.org/cgi/man.cgi?query=amdtemp&apropos=0&sektion=0&manpath=FreeBSD+8.0-RELEASE&format=html)
  - `tmpfs_load="YES"`
    * [Efficient memory file system](https://en.wikipedia.org/wiki/Tmpfs https://www.freebsd.org/cgi/man.cgi?tmpfs(5))
  - `aio_load="YES"`
    * Input and output (I/O) operations on a computer can be extremely slow compared to the processing of data.
    * Asynchronous I/O, or "Non-sequential I/O" is a form of input/output processing that permits other processing
    to continue before the transmission has finished.
    * On Asynchornous I/O see [this](https://en.wikipedia.org/wiki/Asynchronous_I/O) and also
    [this](https://www.freebsd.org/cgi/man.cgi?query=aio&sektion=4)
  - `snd_driver_load="YES"`
    * [More on loading sound subsystem.](https://cooltrainer.org/a-freebsd-desktop-howto/)
  - `hw.psm.synaptics_support=1`
    * [More on synaptic support.](For more options https://wiki.freebsd.org/SynapticsTouchpad)
    
4. Enable everything by setting `vi /etc/rc.conf`
```
moused_enable="YES"
# powerd: hiadaptive speed while on AC power, adaptive while on battery power
powerd_enable="YES"
powerd_flags="-a hiadaptive -b adaptive"
# Reduce power consumption by using higher C-states than the default of C1
performance_cx_lowest="Cmax"
economy_cx_lowest="Cmax"
# Enable BlueTooth
hcsecd_enable="YES"
sdpd_enable="YES"
# Synchronize system time
ntpd_enable="YES"
# Let ntpd make time jumps larger than 1000sec
ntpd_flags="-g"
# Enable hardware detection and interprocess communication.
hald_enable="YES"
dbus_enable="YES"
```
  - `performance_cx_lowest="Cmax"`
    * [Tune power consumption](https://wiki.freebsd.org/TuningPowerConsumption)
  - `sdpd_enable="YES"`
    * [Bluetooth setup](https://www.freebsd.org/doc/handbook/network-bluetooth.html)
  - `hald_enable="YES"`
    * No harm in choosing yes but you can check dependencies via [this help.](http://www.freshports.org/sysutils/hal)
    * **`X.org`** by default uses FreeBSD’s `devd` for hardware detection and D-Bus for interprocess communication.
    * **`HAL`** used to be default but is in the [process of deprecation, so you may not need it unless you plan to use a desktop environment that still depends on it](https://www.freebsd.org/gnome/docs/halfaq.html)
