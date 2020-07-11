# Set-up to Install softwares using the [`portsnap` method.](https://www.freebsd.org/doc/en_US.ISO8859-1/books/handbook/ports-using.html#PORTS-SKELETON)

[Also refer to UCBT_BSD10x_013_Ports_Tree.mp4]

- [`ports` collection](http://www.freebsd.org/doc/en_US.ISO8859-1/books/handbook/ports-using.html#PORTS-SKELETON)
is a directory skeleton of `Makefiles` and patches describing how to programmatically build
packages of third-party software for your computer.
- Each port contains the metadata for that piece of software including the
  * *filename* of the source archive,
  * `sha256` hash of the files,
  * what other software dependencies it requires,
  * what compile-time options are available,
  * what files it installs, and
  * any patches necessary to work around non-portable code
  * or fix issues that can't be upstreamed to the projects themselves.
- Every port has a maintainer whose job it is to keep the port up to date and
respond to issues if they arise with newer versions of the operating system.
- When you make a port in the Ports Collection it
  * downloads the upstream source archive,
  * patches it,
  * configures it,
  * builds a customized binary package, and
  * uses FreeBSD’s underlying binary package manager to install it.

## Setup the [ports](https://www.freebsd.org/ports/) collection.

1. Download compressed snapshot of the `ports` collection into `/var/db/portsnap`
```
portsnap fetch
```

2. Extract the snapshot into `/usr/ports`.
```
portsnap extract
```

3. See the contents of `/usr/ports`.
```
cd /usr/ports && ls
```

4. **Always check the `UPDATING` file before any software updates.**
```
vi UPDATING
```
  - It tracks breaking changes in packages and the steps necessary to to fix them.
  - It will tell you
    * when configuration file syntax has changed,
    * how to fix dependencies when ports split or merge,
    * when default versions of packages change, and more.

5. Update the `ports` collection snapshot.
```
portsnap fetch update
```

6. Install tool (software) to programmatically updrade ports.
  `portsupgrade` is a tool to programmatically upgrade ports (another is `portmaster` but I prefer `portupgrade`).
  
  6.1. Find location of `portupgrade`.
  ```
  whereis portupgrade
  ```
  
  6.2. Enter the directory of the `portupgrade` source code.
  ```
  cd /usr/ports/ports-mgmt/portupgrade
  ```
  
  6.3. Install `portsupgrade`.
  ```
  make install clean choose default settings
  ```
  or simply just
  ```
  pkg install portupgrade
  ```
  
  Third-party softwares installed through the package manager ends up in `/usr/local` where it mirrors the hierarchy of `/usr`.
  *To have them in two places gives a fairly clean separation of the base system from the packages*.
  
  For example, `/usr/local/bin` is where you will find *firefox* after installing `www/firefox`,
  but `/usr/bin` is where you will find `ee` or `sed`.
  
  As you have seen (preceeding markdown instruction files),
  configuration for the operating system is done in `/etc`.
  Configuration files for your ports will usually be in `/usr/local/etc`.
  
  Check the [`hier` manual](https://www.freebsd.org/cgi/man.cgi?query=hier&ektion=&manpath=) for the full layout.
  
  
