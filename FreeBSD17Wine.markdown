# 1. Install [`WINE`](https://wiki.winehq.org/FreeBSD)
## 1.1. Check if you have FreeBSD-32 or FreeBSD-64
```
uname -mrs
```
Output should look something like `FreeBSD 12.1-RELEASE-p1 amd64`.
## 1.2. Install for FreeBSD-32
```
pkg install -y wine wine-devel wine-staging
```
## 1.3. Install for FreeBSD-64
```
pkg install -y i386-wine i386-wine-devel i386-wine-staging
```
Why does these packages have i386 as prefix for 64-bit FreeBSD?

- These packages are only meant to allow 32-bit wine on amd64 systems. If you're on 32-bit FreeBSD, these packages have a failsafe that redirects to 32-bit packages.
- Although FreeBSD can build and run the pure 64-bit version of wine it still doesn't support WoW64 which is necessary for the overwhelming majority of Windows apps, AMD64 systems still need to use the 32-bit package.

# 2. Useful wine commands
[Never run wine as root](https://wiki.winehq/org)
## 2.1. Install `.exe`
```
wine setup.exe
```
## 2.2. Windows Control Panel
```
wine control
```
## 2.3. Windows Add/Remove Programs
```
wine uninstaller
```
## 2.4. f
