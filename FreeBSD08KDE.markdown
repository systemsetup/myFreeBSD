# 1. Install [`kde`](https://kde.org/) windows manager.
Using [`pkg`](https://www.freshports.org/x11-wm/kde5) just do
```
pkg install kde5
```
# 2. Mount `/proc` with `vi /etc/fstab`
```
proc  /proc procfs  rw  0 0
```
# 3. Enable hald (Hardware Abstraction Layer Daemon, provides services to the desktop) and dbus (Desktop Bus, an interprocess communications mechanism)
Enable them in `vi /etc/rc.conf`
```
dbus_enable="YES"
hald_enable="YES"
```
For more on kde refer to the [documentation.](https://www.freebsd.org/doc/handbook/x11-wm.html)

# 2. Set up KDE
```
echo "exec ck-launch-session startplasma-x11" >> .xinitrc
```
However if this does not work
## 2.1. Install SDDM, simple desktop display manager
This is because KDE no longer develops its KDM (KDE Desktop Manager)
```
pkg install -y sddm
```
Then enable it in `vi /etc/rc.conf` with
```
sddm_enable="YES"
```
