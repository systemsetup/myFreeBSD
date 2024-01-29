## [Install Bhyve](https://www.freshports.org/sysutils/vm-bhyve)

Assuming virtualization is enabled in BIOS run the command
- `pkg install vm-bhyve`

Write `vi /etc/rc.conf`
```
vm_enable="YES"
vm_dir="/hdd/vm-bhyve"
```

Run `vm init`
