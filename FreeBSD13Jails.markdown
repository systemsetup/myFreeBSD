# Get up and running jails.
1. Install [ezjail.](https://www.freshports.org/sysutils/ezjail/)
   ```
   pkg install ezjail
   ```

2. Enable ezjail in `vi /etc/rc.conf` file.
   ```
   ezjail_enable=”YES”
   ```

3. Start ezjail.
   ```
   service ezjail start
   ```

4. Change jail storage location from default `/usr/jails`.
  Let us assume you want jails to be soted in `/mysecondhdd/jails`
  - `zfs list`
     ```
     NAME                   USED  AVAIL  REFER  MOUNTPOINT
     bootpool               476K  16.5G    21K  /bootpool
     bootpool/clone          18K  16.5G    18K  /bootpool/clone
     <hostnamehere>         296K  16.5G    19K  /<hostnamehere>
     <hostnamehere>/home    277K  16.5G   277K  /<hostnamehere>/home
     ```
   - mount
     ```
     zfs create –o mountpoint=/mysecondhdd/jails bootpool/jails
     ```
   - d
     ```
     zfs destroy bootpool/jails
     zfs destroy -f bootpool/jails
     zfs destroy -r bootpool/jails
     ```
   - d
     ```
     camcontrol devlist
     ```
   - d
     ```
     zpool status bootpool
     ```
   - d
     ```
     zpool status <hostnamehere>
     ```
   - d
     ```
     zpool create mysecondhdd_pool /dev/ada0
     ```
   - d
     ```
     zpool destroy mysecondhdd_pool
     ```
   - d
     ```
     zpool list
     zfs list
     ```
   - d
     ```
     zfs allow joe create,destroy,mount mysecondhdd_pool
     ```
   - d
     ```
     zfs allow mysecondhdd_pool
     ```
   - d
