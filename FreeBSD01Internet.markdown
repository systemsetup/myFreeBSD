# Internet setup

## 1. WiFi setup

If you don’t have a wired connection but just wireless.
My advice is to setup the wireless connection before anything else.
Login as *root*.

1. To see available hardwares for internet connection
```
ifconfig
ifconfig -l
```
In this case the wireless card is `wlan0` (yours may be `iw0` etc ...).

2. To scan the wireless access points:
```ifconfig wlan0 up scan```
You will see your `ssid` in the first column and its security protocol *wep*, *wpa* etc ...) under the last column.

3. To connect to your network
```ifconfig wlan0 ssid your_ssid_name```

4. Load wifi support by opening the `vi /boot/loader.conf` file.

| Already in file                                      | Add below in file      |
| ---------------------------------------------------- | ---------------------- |
| `geli_ada1p5_keyfile0_load="YES"`                    | `wlan_wep_load="YES"`  |
| `geli_ada1p5_keyfile0_type="ada1p5:geli_keyfile0"`   | `wlan_ccmp_load="YES"` |
| `geli_ada1p5_keyfile0_name="/boot/encryption.key"`   | `wlan_tkip_load="YES"` |
| `aesni_load="YES"`                                   |                        |
| `geom_eli_load="YES"`                                |                        |
| `vfs.root.mountfrom="zfs:neuralgraphs/ROOT/default"` |                        |
| `kern.geom.label.disk_ident.enable="0"`              |                        |
| `kern.geom.label.gptid.enable="0"`                   |                        |
| `zpool_cache_load="YES"`                             |                        |
| `zpool_cache_type="/boot/zfs/zpool.cache"`           |                        |
| `zpool_cache_name="/boot/zfs/zpool.cache"`           |                        |
| `geom_eli_passphrase_prompt="YES"`                   |                        |
| `zfs_load="YES"`                                     |                        |

Press ESC + `:wg` and confirm changes with `cat /boot/loader.conf`

5. Reboot to see the effects.

6. Type
```ping www.google.com```
If you don't see package received CTRL + C to quit the process.

7. View `cat /etc/rc.conf` file

| Already in the file         | Add if you don't see                          |
| --------------------------- | --------------------------------------------- |
| `clear_tmp_enable="YES"`    | `hostname="neuralgraphs"`                     |
| `hostname="neuralgraphs"`   | `wlans_iwn0="wlan0"`                          |
| `wlans_iwn0="wlan0"`        | `ifconfig_wlan0="WPA DHCP"`<sup>&#9768;</sup> |
| `ifconfig_wlan0="WPA DHCP"` |                                               |
| `sshd_enable="YES"`         |                                               |
| `moused_enable="YES"`       |                                               |
| `ntpd_enable="YES"`         |                                               |
| `dumpdev="NO"`              |                                               |
| `zfs_enable="YES"`          |                                               |

  Note:
  - <sup>&#9768;</sup>`ifconfig_wlan0="-ht WPA SYNCDHCP"` if it disconnects frequently.
  - `"wlan0"` is the value for `wlans_iwn0` **regardless** of the wireless name you saw when you typed `ifconfig` (in step-1).
    It just turned out here that they have the same name.
    That is if you saw `ath0` using `ifconfig` you still type `wlans_iwn0="wlan0"` in `/etc/rc.conf` file.

8. View wifi `cat /etc/wpa_supplicant.conf`
```
network={
  ssid="my_net1"
  key_mgmt=NONE
  wep_key0=3456789012
  wep_tx_keyidx=0
  priority=1
}

network={
  ssid="my_net2"
  psk=”1235578899”
  priority=2
}

network={
  ssid=""
  key_mgmt=NONE
  priority
```
  - If your wireless is `wep`
    * `wep_key0` value *should not be in quotes*.
    * set `wep_tx_keyidx=0`
    * `key_mgmt=NONE` cannot be omitted
  - For more on wireless refer to [handbook](https://www.freebsd.org/doc/en_US.ISO8859-1/books/handbook/network-wireless.html)
    and [here](https://srobb.net/fbsdquickwireless.html)
    and [also here.](http://www.thegeekstuff.com/2009/11/ping-tutorial-13-effective-ping-command-examples/?utm_source=feedburner&utm_medium=feed&utm_campaign=Feed%3A+TheGeekStuff+(The+Geek+Stuff))

9. `service netif restart` to apply changes done in step-8.

## 2. Ethernet setup

1. You can use DHCP and SLAAC auto-discovery on most home networks via `vi /etc/rc.conf`
  ```
  #hostname="neuralgraphs"
  # Enable DHCP for em0 (identified as em0 from less /var/run/dmesg.boot) and don't let dhclient block
  background_dhclient="YES"
  ifconfig_em0="DHCP"
  ifconfig_em0_ipv6="inet6 accept_rtadv"
  ```

2. You can bring up the interface and get a DHCP address immediately by issuing
  ```
  ifconfig em0 up
  ```
  and
  ```
  dhclient em0
  ```
  again substituting the name of your own interface (here its `em0`).

Alterntive to 1 and 2 above for GL-iNET

1. Use `ifconfig` to identify the ethernet port name. Mine was igb0
2. Enter below to `vi /etc/rc.conf`
```
hostname="my.server.com"
ifconfig_igb0="DHCP"
```
3. Enter `http://192.168.8.1` in the address bar of a browser.
4. Set-up and get connected.
