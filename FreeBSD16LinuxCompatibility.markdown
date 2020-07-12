# 1. Enable [Linux compatibility](https://www.freebsd.org/doc/handbook/linuxemu-lbc-install.html)
This can be enabled by loading the Linux kernel module `kldload linux` and `kldload linux64` for 64-bit compatibility. To check if it is really loaded see the output for `kldstat`.

## 1.1. Enable at boot time.
Add below to `vi /etc/rc.conf`
```
linux_enable="YES"
```
## 1.2. Mount `linprocfs`
Some linux programs need `linprocfs` to be mounted on `/compat/linux/proc`. This can be done by adding below to `vi /etc/fstab`
```
linprocfs   /compat/linux/proc    linprocfs   rw    0   0
```
## 1.3. Mount `linsysfs`
Some linux programs need `linsysfs` to be mounted on `/compat/linux/sys`. This can be done by adding below to `vi /etc/fstab`
```
linsysfs   /compat/linux/sys    linsysfs   rw    0   0
```
## 1.4. Mount `tmpfs`
Some linux programs need `tmpfs` to be mounted on `/compat/linux/dev/shm`. This can be done by adding below to `vi /etc/fstab`
```
tmpfs   /compat/linux/dev/shm    tmpfs   rw,mode=1777    0   0
```
