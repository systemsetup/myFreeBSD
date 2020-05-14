# Mounting setup.

## Mount external HDD (via usb)
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
