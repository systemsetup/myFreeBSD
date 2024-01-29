## [Install Bhyve](https://www.freshports.org/sysutils/vm-bhyve)

1. Install firmware collection for bhyve, [bhyve UEFI](https://wiki.freebsd.org/bhyve/UEFI)
   ```
   pkg install bhyve-firmware
   ```
2. Install [Grub-emu loader for bhyve](https://www.freshports.org/sysutils/grub2-bhyve)
   ```
   pkg install grub2-bhyve
   ```
1. Run the command
   ```
   pkg install vm-bhyve
   ```
2. Enable it in `vi /etc/rc.conf`
  ```
  vm_enable="YES"
  vm_dir="/hdd/vm-bhyve"
  ```
3. Run
   ```
   vm init
   ```
