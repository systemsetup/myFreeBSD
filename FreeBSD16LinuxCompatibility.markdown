# 1. Enable [Linux compatibility](https://www.freebsd.org/doc/handbook/linuxemu-lbc-install.html)
This can be enabled by loading the Linux kernel module `kldload linux` and `kldload linux64` for 64-bit compatibility. To check if it is really loaded see the output for `kldstat`.

## 1.1. Enable at boot time.
Add below to `vi /etc/rc.conf`
```
linux_enable="YES"
```

