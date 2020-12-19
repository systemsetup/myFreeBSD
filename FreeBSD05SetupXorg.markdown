# Setup Xorg for GUI environment.

## Method-1
The fastest way to install Xorg is using [`pkg`](https://www.freebsd.org/doc/en_US.ISO8859-1/books/handbook/x-install.html).
```
pkg install xorg
```
However, if what you want is flexibility see the method below.

## Method-2
In [`/etc/rc.conf`](https://www.freebsd.org/doc/en_US.ISO8859-1/books/handbook/x-config.html)
you must enable `hald_enable="YES"` and `dbus_enable="YES"`

### Install `xorg`
1. Go to `ports` directory for `xorg`.
```
cd /usr/ports/x11/xorg
```
2. Configure `xorg`.
```
make config-recursive
```
3. Install `xorg`.
```
make install
```
4. Windows should stop popping up but if it does repeat the steps.
```
cd /usr/ports/x11/xorg/ && make rmconfig && make config-recursive reinstall clean
```

### [Configure `xorg`.](https://www.freebsd.org/doc/handbook/x-config.html)
It is important to note that
- Video cards, monitors, and input devices are automatically detected and do not require any manual configuration.
- Do not create `xorg.conf` or run a `-configure` step unless automatic configuration fails.
- Manual configuration is usually not necessary. Please **do not manually create configuration files unless autoconfiguration does not work**.

`Xorg` looks in several directories for configuration files.
```
/usr/local/etc/X11/
```
is the recommended directory for these files on FreeBSD.
Using this directory helps keep application files separate from operating system files.

(*ASIDE:*
Storing configuration files in the legacy /etc/X11/ still works.
However, this mixes application files with the base FreeBSD files and is not recommended.)

Some rule of thumbs for managing the xorg configuration files.
- It is more flexible to *use multiple files that each configure a specific setting*
rather than a single `xorg.conf`.
- These files are stored in the `xorg.conf.d/` subdirectory of the main configuration file directory.
- The full path is typically `/usr/local/etc/X11/xorg.conf.d/`.

1. Check current resolution.
```
xdpyinfo or xwininfo –root or xprop –root
```

2. [Stop using `X –configure`](https://www.freebsd.org/doc/en_US.ISO8859-1/books/handbook/x-config.html)

~~`cd /root && Xorg -configure`~~

This will create a `X.org` configuration file in `/root/xorg.conf.new` based on detected hardware.

3. Check that xorg works.
```
startx
```

4. Create `/etc/X11/xorg.conf` file.
```
vi /etc/X11/xorg.conf
```
And ESC + `:wq`.

5. Move the `xorg.conf.new` (automatically created) and name it `xorg.conf`.
```
mv xorg.conf.new /etc/X11/xorg.conf
```

6. to do
```
pciconf –vl | less    # without less you cannot scroll up
dmesg | grep VGA      # others like CPU think what to type based on pciconf –vl
```

7. Configure the `vi /etc/X11/xorg.conf` file.
```
Section "Extensions"
	Option	"Composite" "Enable"
EndSection
Section "ServerLayout"
	Identifier     "X.org Configured"
	Screen      0  "Screen0" 0 0
	InputDevice    "Mouse0" "CorePointer"
	InputDevice    "Keyboard0" "CoreKeyboard"
	Option         "AIGLX" "true"   # add this line to the ServerLayout section to enable AIGLX
EndSection
Section "Device"
	Option     "AccelMethod" "EXA"  # enable EXA AccelMethod
	Option     "DRI" "true"         # enable DRI
	Identifier  "Card0"
	Driver      "intel"
	BusID       "PCI:0:2:0"
EndSection
Section "Module"
    Load           "glx"
    Load           "dbe"          # enable extension module dbe
    Load           "dri"          # enable extension module dri
    Load           "dri2"         # enable extension module dri2
    Load           "extmod"       # enable extension module extmod
    Load           "record"       # enable extension module record
    Load           "freetype"     # enable font module freetype
    Load           "bitmap"       # enable font module bitmap
    Load           "type1"        # enable font module type1
EndSection
```

8. Install fonts.
```
cd /usr/ports/x11-fonts/urwfonts/  && make install clean BATCH=yes
```
or
```
cd /usr/ports/x11-fonts/webfonts/  && make install clean BATCH=yes
```
[`webfonts` is a collection of free to use TrueType fonts created by Monotype and Microsoft Typography.](http://www.freshports.org/x11-fonts/webfonts)

9. Assign the fonts to `Xorg`.
Assign the installed fonts using the installed font-path in your `/etc/X11/xorg.conf` file.

But note that
- the installed font-paths in `ports` are not entirely standardized.
- most end up in `/usr/local/lib/X11/fonts`,
- some end up in `/usr/local/share/fonts/`, and
- some in just `/usr/local/share/`.

9.1. Check if there are fonts in the prototypical directories
```
cd /usr/local/lib/X11/fonts/ && ls
cd /usr/local/share/fonts/  && ls     [in my case fonts were here]
cd /usr/local/share/   && ls
```
9.2. Add the located font-path to `/etc/X11/xorg.conf` file.
Below is an example `vi /etc/X11/xorg.conf` file.
```
Section "Files"
    ModulePath      "/usr/local/lib/xorg/modules/"
    FontPath        "/usr/local/share/fonts/misc/"
    FontPath        "/usr/local/share/fonts/OTF/"
    FontPath        "/usr/local/share/fonts/TTF/"
    FontPath        "/usr/local/share/fonts/Type1/"
    FontPath        "/usr/local/share/fonts/100dpi/"
    FontPath        "/usr/local/share/fonts/75dpi/"
    FontPath        "/usr/local/share/fonts/cyrillic/"
    FontPath        "/usr/local/share/fonts/dejavu/"
    FontPath        "/usr/local/share/fonts/encodings/"
    FontPath        "/usr/local/share/fonts/webfonts/"
EndSection
```
