Refer to the [documentation.](https://www.freebsd.org/doc/handbook/x11-wm.html)

# 1. Install [`kde`](https://kde.org/) windows manager.
Using [`pkg`](https://www.freshports.org/x11-wm/kde5) just do
```
pkg install kde5
```
## 1.1. Mount `/proc` with `vi /etc/fstab`
```
proc  /proc procfs  rw  0 0
```
## 1.2. Enable hald (Hardware Abstraction Layer Daemon, provides services to the desktop) and dbus (Desktop Bus, an interprocess communications mechanism)
Enable them in `vi /etc/rc.conf`
```
dbus_enable="YES"
hald_enable="YES"
```
## 1.3. Start KDE
### 1.3.1. Command line approach
```
echo "exec ck-launch-session startplasma-x11" >> .xinitrc
```
Then in command line `startx`.
### 1.3.2. Install SDDM, simple desktop display manager
This is because KDE no longer develops its KDM (KDE Desktop Manager)
```
pkg install -y sddm
```
Then enable it in `vi /etc/rc.conf` with
```
sddm_enable="YES"
```
**Tip**
Shortcut to return to console `CTRL`**+**`ALT`**+**`F1`
## 1.4. Troubleshooting
### 1.4.1. Reset KDE Plasma Taskbar (panel with widgets) to default
Delete plasma file and restart.
```
rm -rf ~/.config/plasma-org.kde.plasma.desktop-appletsrc
```
Or you may move the file as a backup and restart.
```
mv ~/.config/plasma-org.kde.plasma.desktop-appletsrc ~/.config/plasma-org.kde.plasma.desktop-appletsrc-old
```
