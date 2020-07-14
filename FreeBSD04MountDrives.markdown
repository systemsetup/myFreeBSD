# Mounting setup.

## Allow access to a variety of disks and input/output devices.
Relax default permissions on the device filesystem to allow normal users access to a variety of disks and input/output devices.

- Permissions for devices existing at boot time are set in `devfs.conf`.
- Each line defines a full device path and octal permission value.
- To see the devices:
  ```
  less /var/run/dmesg.boot
  ```
  and also
  ```
  ls /dev
  ```
- For device name look at [Table 3.3](https://www.freebsd.org/doc/handbook/disk-organization.html)

1. Below shows the `vi /etc/devfs.conf` file allowing access to various disks and input/output devices.
  ```
  # Allow all users to access optical media
  perm    /dev/acd0       0666
  #perm    /dev/acd1      0666
  perm    /dev/cd0        0666
  #perm    /dev/cd1       0666 
  # Allow all two USB Devices to be mounted
  perm    /dev/da0        0666
  perm    /dev/da1        0666
  # Misc other devices
  perm    /dev/pass0      0666              # not sure
  perm    /dev/xpt0       0666              # not sure
  perm    /dev/uscanner0  0666
  perm    /dev/video0     0666                               # permission for webcam
  #perm    /dev/tuner0    0666                               # permission for capturing using tuner as source
  #perm    /dev/dvb/adapter0/demux0    0666                  # permission for demultiplexer for the tuner
  #perm    /dev/dvb/adapter0/dvr       0666                  # part of tuner setup
  #perm    /dev/dvb/adapter0/frontend0 0666                  # part of tuner setup
  ```
  Note:
  - `perm    /dev/uscanner0  0666`
    * Permission for [USB scanners.](https://www.freebsd.org/cgi/man.cgi?query=uscanner&sektion=4&manpath=FreeBSD+5.4-RELEASE)
  - `perm    /dev/tuner0    0666`
    * Permission for [TV tunners.](https://gist.github.com/dreamcat4/32fac8eb6f5db515b68d)
  - `perm    /dev/dvb/adapter0/demux0    0666`
    * Permission for demultiplexer for the tuner. About demultiplexer see [this](https://en.wikipedia.org/wiki/Multiplexer)
    and [this.](http://electronics-course.com/demux)
  - `perm    /dev/pass0      0666`
    * Not sure, see [this](https://www.freebsd.org/cgi/man.cgi?query=pass&sektion=4) and
    [this.](https://www.freebsd.org/doc/en/books/handbook/scanners.html)
  - `perm    /dev/xpt0       0666`
    * [Not sure](https://www.freebsd.org/cgi/man.cgi?query=xpt&sektion=4)

2. For devices that may be connected post-boot, we add an entry to a `devfs.rules` ruleset.
  **Rulesets must have a unique name and number**,
  and their rules are composed of a path or quoted path [glob](https://en.wikipedia.org/wiki/Glob_(programming))
  and octal permission value.
  
  2.1. Below shows the `vi /etc/devfs.rules`
  ```
  [devfsrules_common=7]
  add path 'ad[0-9]\*'	mode 0660   group   operator
  add path 'ada[0-9]\*'	mode 0660   group   operator
  add path 'da[0-9]\*'	mode 0660   group   operator
  add path 'acd[0-9]\*'	mode 0660   group   operator
  add path 'cd[0-9]\*'	mode 0660   group   operator
  add path 'mmcsd[0-9]\*'    mode 0660   group   operator
  add path 'pass[0-9]\*'	mode 0660   group   operator
  add path 'xpt[0-9]\*'	mode 0660   group   operator
  add path 'ugen[0-9]\*'	mode 0660   group   operator      # generic usb support
  add path 'usbctl'	      mode 0660   group   operator
  add path 'usb/\*'	      mode 0660   group   operator
  add path 'lpt[0-9]\*'     mode 0660   group   operator    # generic printer device driver
  add path 'ulpt[0-9]\*'	mode 0660   group   operator      # USB printer support
  add path 'unlpt[0-9]\*'	mode 0660   group   operator      # in case of problems with ulpt
  add path 'fd[0-9]\*'	mode 0660   group   operator        # file descriptor files accessible through the file system
  add path 'uscan[0-9]\*'	mode 0660   group   operator      # scan upstream sources for new released of software
  add path 'video[0-9]\*'	mode 0660   group   operator
  #add path 'tuner[0-9]*'   mode 0660   group   operator
  #add path 'dvb/\*'	      mode 0660   group   operator     # for Digital TV (DVB, ATSC)
  #add path 'cx88*'         mode 0660   group   operator     # video capture driver
  #add path 'cx23885*' mode 0660                             # CX23885-family stream configuration device
  #add path 'iicdev*'       mode 0660   group   operator     # for inter IC devices
  #add path 'uvisor[0-9]*'  mode 0660   group   operator     # USB support for the PalmOS based PDAs
  ```
  Note:
  - `add path 'ugen[0-9]\*'	mode 0660   group   operator`
    * Permission rule for [generic usb support](https://www.freebsd.org/cgi/man.cgi?ugen)
  - `add path 'lpt[0-9]\*'     mode 0660   group   operator`
    * Permission rule for [generic printer device driver](https://www.freebsd.org/cgi/man.cgi?query=lpt&sektion=4)
  - `add path 'ulpt[0-9]\*'	mode 0660   group   operator`
    * Permission rule for [usb printer support](https://www.freebsd.org/cgi/man.cgi?query=ulpt&sektion=4)
  - `add path 'fd[0-9]\*'	mode 0660   group   operator`
    * [File descriptor files](https://www.freebsd.org/cgi/man.cgi?query=fd&sektion=4)
    which can be accessed through the file system.
  - `add path 'uscan[0-9]\*'	mode 0660   group   operator`
    * [Scan upstream sources for new released of software](https://www.opennet.ru/man.shtml?topic=uscan&category=1&russian=2)
  - `add path 'cx88*'         mode 0660   group   operator`
    * [Video capture driver](http://corona.homeunix.net/cx88wiki)
    also [see.](https://gist.github.com/dreamcat4/d28a6daf78b1ad2ec663)
  - `add path 'cx23885*' mode 0660`
    * CX23885-family stream configuration device
    * [cx23885 webcamd driver](https://manned.org/cx23885/1bceb5d1) for TV tuner boards.
  - `add path 'iicdev*'       mode 0660   group   operator`
    * Permission rule for inter [IC devices.](http://www.i2c-bus.org/i2c-bus/)
    * Also see [this](https://en.wikipedia.org/wiki/I%C2%B2C) and
    [this.](http://dieterbsd.blogspot.fr/2008/03/hdtv-capture-driver-for-cx2388x.html)
  - `add path 'uvisor[0-9]*'  mode 0660   group   operator`
    * [USB support](https://www.freebsd.org/cgi/man.cgi?query=uvisor) for the PalmOS based PDAs
    * Also see [this.](https://www.freebsd.org/cgi/man.cgi?query=uvisor&sektion=4&manpath=FreeBSD+5.2-RELEASE)
  
  2.2. Enable the ruleset in `vi /etc/rc.conf`
  ```
  devfs_system_ruleset="devfsrules_common"
  ```

## Mount external HDD (via usb).
The [`procfs`](https://www.freebsd.org/cgi/man.cgi?query=procfs&sektion=&n=1) and
[`fdescfs`](http://www.manpages.info/freebsd/fdesc.5.html)
virtual filesystems are not a default part of BSD
but they are frequently required for compatibility with programs and environments
written with Linux in mind, such as GNOME/MATE and KDE.

The FreeBSD equivalent is `sysctl`, but you can mount `/proc` too if you plan to use software requiring it.

Some special filesystems like `fdescfs` must be mounted late on ZFS-rooted systems because
the location of their mountpoint won't exist until late in the boot process.

1. `vi /etc/fstab` file will set the `procfs` and `fdescfs` filesystem.
```
proc	/proc	   procfs	rw	       0	0
fdesc	/dev/fd	   fdescfs	rw,auto,late	0	0
```
  Note:
  - `fstab` (`/etc/fstab`) is of the [format](https://www.freebsd.org/doc/handbook/mount-unmount.html):
```
device      /mount-point      fstype      options      dumpfreq     passno
```
  - [On disk names.](https://www.freebsd.org/doc/handbook/disk-organization.html#disks-naming)
  
2. If you neglected to add your personal user account to the `wheel` and `operator` groups during installation,
now is a good time to do so. In this example my user is *joe*.
  - If it not done already done assigning `wheel` membership lets you use `su` to become `root`
  ```
  pw usermod joe -G wheel
  ```
  - If the `operator` membership is required for device permissions in this configuration do
  ```
  pw usermod joe -G operator
  ```
  - [More on `pw` formatting.](https://www.freebsd.org/cgi/man.cgi?query=pw&sektion=8&manpath=freebsd-release-ports)

3. Confirm that the username (here, *joe*) is in both `wheel` and `operator` groups.

  | Contents of the `cat /etc/group` will be of the format | This should be corrected to |
  | ------------------------------------------------------ | --------------------------- |
  | `wheel:*:root`                                         | `wheel:*:root,joe`          |
  | `...`                                                  | `...`                       |
  | `operator:*:5:root`                                    | `operator:*:5:root,joe`     |
  | `...`                                                  | `...`                       |
  
4. Ensure that `operator` is able to read and write the device by writing the [ruleset below for device configuration](https://www.freebsd.org/doc/handbook/usb-disks.html)
  - write the ruleset below in `vi /etc/devfs.rules`
  ```
  [localrules=5]
  add path 'da*' mode 0660 group operator
  ```
  
  - Why `da` for usb? Look at [Table 3.3](https://www.freebsd.org/doc/handbook/disk-organization.html) for device name.
    
  - enable the ruleset in `vi /etc/rc.conf`
  ```
  devfs_system_ruleset="localrules"
  ```
  
  - [let users mount the disk](https://www.freebsd.org/doc/handbook/usb-disks.html) by enabling it in `vi /etc/sysctl.conf`
  ```
  vfs.usermount=1
  ```

## [Add a second HDD](https://www.freebsd.org/doc/handbook/disks-adding.html)

1. [Identify the device.](https://forums.freebsd.org/threads/45411/)
```
less /var/run/dmesg.boot
```
Here it was identified as `ada0`

2. Wipe the HDD clean
```
gpart destroy –F ada0
```

3. Create the GPT partition scheme and then add a single partition.
```
gpart create -s GPT ada0
```

4. To improve performance on newer disks with larger hardware block sizes, the partition is aligned to one megabyte boundaries.
```
gpart add -t freebsd-ufs -a 1M ada0
```

5. See disk partition information.
```
gpart show ada0
```

6. Create file system in the new partition on the new disk.
```
newfs –U /dev/ada0p1
```

7. Create an empty directory as a **mountpoint**; a location for mounting the new disk in the original disk's file system.
```
mkdir /my_second_hdd
```

8. Add an entry to `/etc/fstab` so that the new disk will be mounted automatically at startup.
```
/dev/ada0p1	  /my_second_hdd	  ufs	  rw	  2	  2
```

## Mount usb drives.

### Setup to mount usb drives (especially for non-root users) [*Carefully* read the pre-requisite.](https://www.freebsd.org/doc/en_US.ISO8859-1/books/handbook/usb-disks.html)

1. List the HDD's and any usb-pen drive connected.
```
camcontrol devlist
```

2. List the connected usb-pen drives, i.e. all the connected `da`'s, i.e., `da*`.
```
ls /dev/da*
```

3. Change owner (`chown`); below gives username (*joe*) in the group (`operator`) the folder (*/flashmedia*) access.
```
chown    joe:operator    /mnt/flashmedia
```
  [Note that the group name is `operator` because `/etc/devfs.rules` has the line](https://www.freebsd.org/doc/en_US.ISO8859-1/books/handbook/usb-disks.html)
  ```
  add path 'da[0-9]\*'    mode 0660   group   operator
  ```

### [Mount connected usb-pendrive (root or normal user)](http://www.unix.com/shell-programming-and-scripting/135969-unmount-usb-disk.html)

1. [Get the name of the connected usb.](https://forums.freebsd.org/threads/25079/)
```
ls /dev/da*
```
Here it was identified as `da0s1`.

2. [Get the format type of the usb named `da0s1`.](https://forums.freebsd.org/threads/25079/)
```
file –s /dev/da0s1
```

3. Mounts the **FAT** formatted `da0s1` usb to the */flashmedia* folder.
```
mount_msdosfs /dev/da0s1 /mnt/flashmedia
```

4. For [NTFS](http://kflu.github.io/2018/02/03/2018-02-03-freebsd-ntfs/)
```
ntfs-3g /dev/da0s1 /mnt/flashmedia
```

5. Unmount the usb device.
```
umount /mnt/flashmedia
```


