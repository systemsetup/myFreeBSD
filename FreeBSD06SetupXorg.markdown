# Setup Xorg for GUI environment.

cd /usr/ports/x11/xorg
make config-recursive
make install
cd /usr/ports/x11/xorg/ && make rmconfig && make config-recursive reinstall clean [again, no more windows should appear but if does select them and repeat this step until no more window appears]

Note in /etc/rc.conf you must enable hald_enable="YES" dbus_enable="YES"

https://www.freebsd.org/doc/en_US.ISO8859-1/books/handbook/x-config.html
Video cards, monitors, and input devices are automatically detected and do not require any manual configuration. Do not create xorg.conf or run a -configure step unless automatic configuration fails.
Manual configuration is usually not necessary. Please do not manually create configuration files unless autoconfiguration does not work.
Xorg looks in several directories for configuration files. /usr/local/etc/X11/ is the recommended directory for these files on FreeBSD. Using this directory helps keep application files separate from operating system files.
Storing configuration files in the legacy /etc/X11/ still works. However, this mixes application files with the base FreeBSD files and is not recommended.
It is more flexible to use multiple files that each configure a specific setting than the traditional single xorg.conf. These files are stored in the xorg.conf.d/ subdirectory of the main configuration file directory. The full path is typically /usr/local/etc/X11/xorg.conf.d/.
xdpyinfo or xwininfo –root or xprop –root   [check current resolution]
https://www.freebsd.org/doc/handbook/x-config.html
cd /root
Xorg -configure   [this will create a X.org configuration file in /root/xorg.conf.new based on detected hardware] Stop using X –configure [https://www.freebsd.org/doc/en_US.ISO8859-1/books/handbook/x-config.html]
startx [check that xorg works]
vi /etc/X11/xorg.conf  [just :wq to create the file and folder X11 in /etc]
mv xorg.conf.new /etc/X11/xorg.conf  [now move xorg.conf.new and also give it the name xorg.conf]
pciconf –vl | less    [without less you cannot scroll up]
dmesg | grep VGA  [others like CPU think what to type based on pciconf –vl]
vi /etc/X11/xorg.conf
Section "Extensions"
	Option	"Composite" "Enable"
EndSection
Section "ServerLayout"
	Identifier     "X.org Configured"
	Screen      0  "Screen0" 0 0
	InputDevice    "Mouse0" "CorePointer"
	InputDevice    "Keyboard0" "CoreKeyboard"
	Option         "AIGLX" "true"   [add this line to the ServerLayout section to enable AIGLX]
EndSection
Section "Device"
	Option     "AccelMethod" "EXA"         [enable EXA AccelMethod]
	Option     "DRI" "true"                [enable DRI]
	Identifier  "Card0"
	Driver      "intel"
	BusID       "PCI:0:2:0"
EndSection
Section "Module"
    Load           "glx"
    Load           "dbe"          [enable extension module dbe]
    Load           "dri"          [enable extension module dri]
    Load           "dri2"         [enable extension module dri2]
    Load           "extmod"       [enable extension module extmod]
    Load           "record"       [enable extension module record]
    Load           "freetype"   [enable font module freetype]
    Load           "bitmap"       [enable font module bitmap]
    Load           "type1"        [enable font module type1]
EndSection

cd /usr/ports/x11-fonts/urwfonts/    
cd /usr/ports/x11-fonts/webfonts/    [collection of free to use TrueType fonts created by Monotype and Microsoft Typography http://www.freshports.org/x11-fonts/webfonts]
make install clean BATCH=yes

After installing the fonts you need to tell Xorg about them using a FontPath in your /etc/X11/xorg.conf. But the font paths in Ports are not entirely standardized. Most end up in /usr/local/lib/X11/fonts, some end up in /usr/local/share/fonts/, and some in just /usr/local/share/. Here’s the relevant section from my xorg.conf. Yours should look similar.
So first check: if there is a fonts folder, check if there are fonts in the folder using the ls command in-
cd /usr/local/lib/X11/fonts/
cd /usr/local/share/fonts/     [in my case fonts were here]
cd /usr/local/share/
Then add them to the /etc/X11/xorg.conf file
vi /etc/X11/xorg.conf
Section "Files"
    ModulePath        "/usr/local/lib/xorg/modules"
    FontPath        "/usr/local/share/fonts/misc/"
    FontPath        "/usr/local/share/fonts/OTF/"
    FontPath        "/usr/local/share/fonts/TTF/"
    FontPath        "/usr/local/share/fonts/Type1/"
    FontPath        "/usr/local/share/fonts/100dpi/"
    FontPath        "/usr/local/share/fonts/75dpi/"
    FontPath        "/usr/local/share/fonts/cyrillic/"
    FontPath        "/usr/local/share/fonts/dejavu/"
    FontPath        "/usr/local/share/fonts/encodings/"
    FontPath        "/usr/local/share/font/util/"
    FontPath        "/usr/local/share/font/webfonts/"
EndSection
