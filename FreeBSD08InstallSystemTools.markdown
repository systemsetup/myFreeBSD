# Install System Tools
1. If you did not manage to install the [FreeBSD Handbook](https://www.freshports.org/misc/freebsd-doc-en)
```
pkg install en-freebsd-doc
```
Also see this [discussion](https://forums.freebsd.org/threads/how-to-install-handbook.45218/) and refer to FreeBSD [books and articles](https://www.freebsd.org/docs/books.html).

2. [`git`](https://www.freshports.org/devel/git/), [`wget`](https://www.freshports.org/ftp/wget), [7zip](https://www.freshports.org/archivers/p7zip/)
```
pkg install -y git wget p7zip
```
To invoke/call 7zip type `7z`.

3. [Simple X terminal](https://www.freshports.org/x11/sterm)
```
pkg install sterm
```
[st is a minimalistic X terminal.](https://st.suckless.org/)

4. Optionally, [`rox-filer`](http://www.freshports.org/x11-fm/rox-filer) graphical file manager for X11.
```
pkg install rox-filer
```

## Some applications aimed at [Window Maker](http://windowmaker.org/) GUI environment.

1. [`wmclock`](http://www.freshports.org/x11-clocks/wmclock) applet which displays the date and time in a dockable tile in the same style as the clock specially designed for the Window Maker.
```
cd /usr/ports/x11-clocks/wmclock/ && make install clean
```

2. [`wmcpuload`](http://www.freshports.org/sysutils/wmcpuload) applet that displays the current cpu-load in a simulated lcd-display.
```
cd /usr/ports/sysutils/wmcpuload/ && make install clean
```

3. [`wmnd`](http://www.freshports.org/net/wmnd) customizable network monitoring dockapp for Window Maker.
```
cd /usr/ports/net/wmnd/ && make install clean
```
