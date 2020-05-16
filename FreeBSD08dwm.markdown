# 1. Install [`dwm`](https://dwm.suckless.org/), a simple windows manager.
Using [`pkg`](https://www.freshports.org/x11-wm/dwm) just do
```
pkg install dwm
```
Alternatively,
```
cd /usr/ports/x11-wm/dwm/ && make install clean
```

# 2. Set it up.
echo "exec dwm" >> .xinitrc
