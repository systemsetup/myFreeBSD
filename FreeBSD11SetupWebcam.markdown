# Install webcam application and setup the webcam.

1. Install [`webcamd](http://www.freshports.org/multimedia/webcamd); a daemon that enables use of hundreds of different USB based webcam and DVB USB devices.
```
cd /usr/ports/multimedia/webcamd/ && make install clean
```

2. Install [`cuse4bsd-kmod`](http://www.freshports.org/multimedia/cuse4bsd-kmod); webcamd depends on this kernel module which allows
userland programs to create character devices under `/dev`.
```
cd /usr/ports/multimedia/cuse4bsd-kmod/ && make install clean
```

3. Enable the webcam in the `vi /etc/rc.conf` file.
```
# Webcam daemon
webcamd_enable="YES"
vi /boot/loader.conf
# Userland character device driver for webcams
cuse4bsd_load="YES"
```

4. [`pcview`](http://www.freshports.org/multimedia/pwcview); application that you can use to view the video stream of your webcam.
```
cd /usr/ports/multimedia/pwcview/ && make install clean
```
