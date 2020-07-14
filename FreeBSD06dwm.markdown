# 1. Install [`dwm`](https://dwm.suckless.org/), a simple windows manager.
Using [`pkg`](https://www.freshports.org/x11-wm/dwm) just do
```
pkg install dwm
```
Alternatively,
```
cd /usr/ports/x11-wm/dwm/ && make install clean
```
Or,
```
pkg install kde5
```
For KDE then mount `/proc` with `vi /etc/fstab`
```
proc  /proc procfs  rw  0 0
```
Also `vi /etc/rc.conf`
```
dbus_enable="YES"
hald_enable="YES"
```
For more on kde refer to the [documentation.](https://www.freebsd.org/doc/handbook/x11-wm.html)

# 2. Set it up.
## 2.1. If its not already done install [`slim`](https://www.freshports.org/x11/slim)
```
pkg install slim
```
## 2.2. For `dwm`
```
echo "exec dwm" >> .xinitrc
```
## 2.3. For KDE
```
echo "exec ck-launch-session startplasma-x11" >> .xinitrc
```
If this does not work install SDDM because KDE no longer develops its KDM (KDE Desktop Manager)
```
pkg install -y sddm
```
Then enable it in `vi /etc/rc.conf` with
```
sddm_enable="YES"
```
