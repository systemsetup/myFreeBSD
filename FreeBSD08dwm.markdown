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

# 2. Set it up.
## 2.1. If its not already done install [`slim`](https://www.freshports.org/x11/slim)
```
pkg install slim
```
## 2.2. 
echo "exec dwm" >> .xinitrc
