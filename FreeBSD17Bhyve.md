## [Install Bhyve](https://www.freshports.org/sysutils/vm-bhyve)

1. Install firmware collection for bhyve, [bhyve UEFI](https://wiki.freebsd.org/bhyve/UEFI)
   ```
   pkg install bhyve-firmware
   ```
2. Install [Grub-emu loader for bhyve](https://www.freshports.org/sysutils/grub2-bhyve)
   ```
   pkg install grub2-bhyve
   ```
3. Enable virtualization support in `vi /boot/loader.conf`
   ```
   vmm_load=”YES”
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

## Virtualize OS

### [Linux](https://docs.freebsd.org/en/books/handbook/virtualization/#virtualization-bhyve-linux) (Fedora)

1. Load bhyve kernel
   ```
   kldload vmm
   ```
   * load at boot time in `vi /boot/loader.conf`
     ```
     vmm_load="YES"
     nmdm_load="YES"
     if_tap_load="YES"
     if_bridge_load="YES"
     ```
2. Configure the host system
   * create a tap interface for the network device in the virtual machine to attach to
     ```
     ifconfig tap0 create
     ```
     - then configure it
       ```
       sysctl net.link.tap.up_on_open=1
       ```
       + make it permanent in `vi /etc/sysctl.conf`
         ```
         net.link.tap.up_on_open=1
         ``` 
   * create a bridge interface for the network device to participate in the network
     ```
     ifconfig bridge0 create
     ```
     - then configure it to contain the tap interface and the physical interface (here, `igb0`)
       ```
       ifconfig bridge0 addm igb0 addm tap0
       ```
       + the available physical interface can be seen using
         ```
         ifconfig
         ifconfig -l
         ```
     - set the interface name to `igb0bridge` for ease of identification
       ```
       ifconfig bridge0 name igb0bridge
       ```
     - turn it on
       ```
       ifconfig igb0bridge up
       ```
   * set and activate network devices at boot in `vi /etc/rc.conf`
     ```
     # byhve and jail settings bridges 
     cloned_interfaces="bridge0 tap0"
     ifconfig_bridge0_name="igb0bridge"
     ifconfig_igb0bridge="addm igb0 addm tap0 up"
     ```
3. Create a file to use as the virtual disk for the guest machine
   ```
   truncate -s 16G myFedora.img
   ```
   * [`truncate`](https://man.freebsd.org/cgi/man.cgi?query=truncate&sektion=1&format=html) creates the virtual disk storage.
   * Here, 16 GB volume is used.
4. Create `vi device.map` (to map virtual devices to the files of the host system using [sysutils/grub2-bhyve](https://www.freshports.org/sysutils/grub2-bhyve))
   ```
   (hd0) ./myFedora.img
   (cd0) ./some_Fedora_version.iso
   ```
6. ```
   bhyve -AHP -s 0:0,hostbridge -s 1:0,lpc \
   -s 2:0,virtio-net,tap0 \
   -s 3:0,virtio-blk,/hdd/vm-bhyve/Fedora.img \
   -s 4:0,ahci-cd,/hdd/vm-bhyve/Fedora-Workstation-Live-x86_64-39-1.5.iso -c 4 -m 16384M \
   -l bootrom,/usr/local/share/uefi-firmware/BHYVE_UEFI_CSM.fd \
   guest
   ```
5. Start the virtual machine (pre-installed Linux)
   * Load the kernel from the iso using grub
     ```
     grub-bhyve -m device.map -r cd0 -M 1024M linuxguest
     ```
     - 1 GB RAM is allocated to the guest machine with the name "linuxguest"
     - This will start grub.
     - A menu will be displayed if the installation CD (iso) contains a grub.cfg
       + Otherwise, manually locate and load the files, `vmlinuz` and `initrd`
         ```
         grub> ls
         (hd0) (cd0) (cd0,gpt3) (cd0,gpt2) (cd0,gpt1) (host)
         grub> ls (cd0)/images/pxeboot/
         initrd.img vmlinuz
         grub> linux (cd0)/images/pxeboot/vmlinuz
         grub> initrd (cd0)/images/pxeboot/initrd.img
         grub> boot
         ```
      - Note that the option `-M` accepts allocated memory size in MB
        | in GB | in MB |
        | ----- | ----- |
        | 1     | 1024  |
        | 2     | 2048  |
        | 4     | 4096  |
        | 6     | 6144  |
        | 8     | 8192  |
        | 16    | 16384 |
        | 32    | 32768 |
        + More conversions [here](https://www.flightpedia.org/convert/data-storage.html)
   * Start the guest machine
     ```
     bhyve -A -H -P -s 0:0,hostbridge -s 1:0,lpc -s 2:0,virtio-net,tap0 -s 3:0,virtio-blk,./myFedora.img \
     -s 4:0,ahci-cd,./some_Fedora_version.iso -l com1,stdio -c 4 -m 1024M linuxguest
     ```
     - The system will boot and start the installer.
     - After installation, reboot the virtual machine. This will cause bhyve to exit.
6. Destroy the virtual machine instance
   ```
   bhyvectl --destroy --vm=linuxguest
   ```
   * Before restarting the virtual machine its instance must be destroyed
7. Start the virtual machine (post-installed Linux)
   * Load the kernel
     ```
     grub-bhyve -m device.map -r hd0,msdos1 -M 1024M linuxguest
     grub> ls
     (hd0) (hd0,msdos2) (hd0,msdos1) (cd0) (cd0,msdos1) (host)
     (lvm/VolGroup-lv_swap) (lvm/VolGroup-lv_root)
     grub> ls (hd0,msdos1)/
     lost+found/ grub/ efi/ System.map-2.6.32-431.el6.x86_64 config-2.6.32-431.el6.x86_64
     symvers-2.6.32-431.el6.x86_64.gz vmlinuz-2.6.32-431.el6.x86_64
     initramfs-2.6.32-431.el6.x86_64.img
     grub> linux (hd0,msdos1)/vmlinuz-2.6.32-431.el6.x86_64 root=/dev/mapper/VolGroup-lv_root
     grub> initrd (hd0,msdos1)/initramfs-2.6.32-431.el6.x86_64.img
     grub> boot
     ```
   * Start the machine
     ```
     bhyve -A -H -P -s 0:0,hostbridge -s 1:0,lpc -s 2:0,virtio-net,tap0 \
     -s 3:0,virtio-blk,./myFedora.img -l com1,stdio -c 4 -m 1024M linuxguest
     ```
     - The system will boot and present a login prompt.
     - When finished using the virtual machine destroy the instance
       ```
       bhyvectl --destroy --vm=linuxguest
       ```
