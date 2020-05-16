## 1. Install [`slim`](http://www.freshports.org/x11/slim)
[Slim](http://www.freshports.org/x11/slim) is a **desktop-independent** graphical login manager for X11.
The simplest way is
```
pkg install slim
```
Alternatively,
```
cd /usr/ports/x11/slim/ && make install clean
```
## 2. Setup `xinitrc`
Even if you have already installed your favourite windows manager you will not see it unless
you set it up using the file `.xinitrc`.

1. The `.xinitrc` may already exits as `/usr/local/etc/X11/xinit/xinitrc`
   - Notice that this **does not** have a dot in the file name.
   - **Do not use this.**
2. In `/root` directory, create a new
   ```
   vi ~/.xinitrc
   ```
   - after logging in and typing `startx` on text-mode
   - `xinit` will execute `/root/.xinitrc`

### 2.1. Setup `.xinitrc` for [Window Maker](http://windowmaker.org)
Enter the following in the `vi ~/.xinitrc` file
```
#exec $1
#xscreensaver -no-splash &
xdg-user-dirs-update &
redshift &
compton &
exec wmaker
```
Note that
- `exec $1`
   * This is for cases when >1 desktop environments are installed.
   * To identify the installed desktop environments
     ```
	 cd /usr/local/share/xsessions/ && ls
     ```
	 This will return a list, for e.g. `wmake.desktop`, etc ...
	 (How to create a `wmake.desktop` file? see below.)
   * Once you are in slim login manager press **F1** key to toggle between environments.
- `exec wmaker`
   * Putting this in first line means others **below it will not be executed**.
- `xdg-user-dirs-update`
   * Assuming [this](https://www.freshports.org/devel/xdg-user-dirs/) was installed.
- `redshift`
   * Assuming [this](http://www.freshports.org/accessibility/redshift) was installed.
- `compton`
   * Assuming [this](http://www.freshports.org/x11-wm/compton) was installed.
   
### 2.2. Setup `.xinitrc` for [dwm](https://www.freshports.org/x11-wm/dwm)
```
echo "exec dwm" >> ~/.xinitrc
```

## 3. Configure slim for graphical login.
1. Go to
   ```
   cd /usr/local/share/ && mkdir xsessions
   ```
2. Open `vi /usr/local/etc/slim.conf` file.
3. Add below in the `slim.conf` file.
   ```
   sessiondir /usr/local/share/xsessions/
   ```
   - `ports` install desktop entries **usually** in the path
      ```
	  /usr/local/share/xsessions
	  ```
   - above configures the `sessiondir` directive to define the above path.
4. `.xsession` (unlike `.xinitrc`) is executed after logging in graphical-mode and
   then invokes custom session.
   - *X session* will use one of the virtual
   [ttys, terminal initialization information](https://www.freebsd.org/cgi/man.cgi?query=ttys&ektion=&manpath=).
5. Create a desktop entry for your window manager of choice.
   - Eg: **Create a `wmaker.desktop`**
     * In `vi /usr/local/share/xsessions/wmaker.desktop` file enter the following
	   ```
	   [Desktop Entry]
	   Encoding=UTF-8
	   Name=Window Maker
	   Exec=/usr/local/bin/wmaker
	   Comment=This session logs you into Window Maker
	   Type=Application
	   ```
6. Enable graphical login.
   - In the [`vi /etc/rc.conf`](http://daemon-notes.com/articles/desktop/slim) file enter the following
     ```
	 # Enable graphical login
	 slim_enable="YES"
	 ```
	  * This will allow the [initialization of a graphical session](https://www.freebsd.org/cgi/man.cgi?query=slim&sektion=1&manpath=freebsd-release-ports)
	  by entering username and password in a login screen.
	  * If there are >1 desktop environments, e.g. `abc.desktop`, `def.desktop`,
	    toggle between them pressing the **F1** key.
		
7. Useful commands.
   - Even after all the above setup every now and then your computer may start in text-mode
     and not graphical-mode.
	 Then initiate slim in text-mode with
	 ```
	 service slim start
	 ```
   - Alternatively, the command to [force from graphical-mode to text-console](https://lists.freebsd.org/pipermail/freebsd-x11/2005-April/001852.html)
     press the **CTRL + Alt + F1** key.

## 4. Allow non-root to be able to login in with SLiM.
Because the `~/.xinitrc` file is in the `/root` directory SLiM login is only as root.
   - To allow SLim login for other users does
     ```
     cp ~/.xinitrc /usr/home/lungsi/.xinitrc
     ```
   - To find list of users or name of users
     ```
	 cd /usr/home/ && ls
	 ```
