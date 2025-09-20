## 1. Office stuffs
### 1.1. Install [`maxima`](https://www.freshports.org/math/maxima/) and [`wxmaxima`](https://www.freshports.org/math/wxmaxima/), a symbolic mathematics program
```
pkg install -y maxima wxmaxima
```
### 1.2. Install [`dia`](https://www.freshports.org/graphics/dia/); a Viso-like diagramming tool
```
pkg install dia
```
### 1.3. Install [`vim`](https://www.freshports.org/editors/vim/) and [`tmux`](https://www.freshports.org/sysutils/tmux/)
```
pkg install -y vim tmux
```
### 1.4. Install `latex`
For installing the [full Texlive package](https://www.freshports.org/print/texlive-full/)
```
pkg install texlive-full
```
Alternatively, especially if you are installing KDE then you may choose [Kile, KDE Integrated LaTeX Editing Environment](https://kile.sourceforge.io/) from [`editors/kile/`](https://www.freshports.org/editors/kile/)
```
pkg install kile
```

### 1.5. Install [`mupdf`](https://www.freshports.org/graphics/mupdf/); a lightweight PDF, XPS, and E-book viewer.
```
pkg install mupdf
```
### 1.6. Install [`zotero`](https://www.freshports.org/science/zotero); reference management software to manage bibliographic data.
```
pkg install zotero
```
### 1.7. Install [`mutt`](https://www.freshports.org/mail/mutt)
```
pkg install mutt
```
To connect to gmail you will also need [openssl](https://www.freshports.org/security/openssl/)
```
pkg install -y openssl
```
### 1.8. Install [`homebank`](https://www.freshports.org/finance/homebank/)
```
pkg install -y homebank
```
### 1.9. Install [`zim`](https://www.freshports.org/deskutils/zim)
```
pkg install -y zim
```
### 1.10. Install [`kalzium`](https://www.freshports.org/science/kalzium/)
```
pkg install -y kalzium
```
### 1.11. Install [`transmission`](https://www.freshports.org/net-p2p/transmission/)
```
pkg install -y transmission
```

### 1.12. Install [`geany`](https://www.freshports.org/devel/geany/) and [`geany-plugin-markdown`](https://www.freshports.org/devel/geany-plugin-markdown/)
```
pkg install -y geany geany-plugin-markdown
```

### 1.13. Install [`R`](https://www.freshports.org/math/R/) and [`RStudio`](https://www.freshports.org/devel/RStudio/)
```
pkg install -y R RStudio
```

## 2. Programming Environment
### 2.1. Clojure
#### 2.1.1. Install [Leiningen](https://www.freshports.org/devel/leiningen/)
```
pkg install leiningen
```
But as of this writing [Leiningen](https://www.freshports.org/devel/leiningen/) depends on OpenJDK8.
However, some Clojure projects may depend on packages (Clojure or Java packages) that uses Java classes
that require newer Java compilers. Then, install [Leiningen](https://leiningen.org/) without using the`pkg` approach.

* Install the [latest OpenJDK](https://github.com/systemsetup/myFreeBSD/blob/master/FreeBSD10InstallBrowserAndJava.md#install-java--browser)
* Set the default version to the [latest version](https://github.com/systemsetup/myFreeBSD/blob/master/FreeBSD10InstallBrowserAndJava.md#change-default-java-version)
* Download the [lein script](https://raw.githubusercontent.com/technomancy/leiningen/stable/bin/lein)
* As superuser copy the lein script to path `/usr/local/bin/`
  
  ```cp /home/lungsi/Downloads/lein /usr/local/bin/```
* Set it to be executable
  
  ```chmod a+x /usr/local/bin/lein```
* Run the lein script as a normal user
  
  ```/usr/local/bin/lein```

#### 2.1.2. Install [`intellij IDEA`](https://www.freshports.org/java/intellij/)
```
pkg install intellij
```
*NOTE*: To setup for REPL, **File > Project Structure > Modules** and under **Module SDK** pull-down menu pick **Add SDK > JDK** and choose the installed location of openjdkX. To find where openjdkX is installed use `pkg info`; example, `pkg info openjdk17` or `pkg info -f openjdk17`.

### 2.2. Python
#### 2.2.1. Install [`PyCharm`](https://www.freshports.org/devel/pycharm-ce/)
```
pkg install pycharm-ce
```

### 2.3. Rust
#### 2.3.1. Install [rust](https://www.freshports.org/lang/rust)
```
pkg install rust
```
#### 2.3.2. Install [`RustRover`](https://www.freshports.org/devel/jetbrains-rustrover/)
```
pkg install jetbrains-rustrover
```
### 2.3. Docker environment
#### 2.3.1. Install [docker client](https://www.freshports.org/sysutils/docker/)
```
pkg install docker
```
#### 2.3.2. Install [`docker-machine`](https://www.freshports.org/sysutils/docker-machine/)
```
pkg install docker-machine
```
## 3. Graphic Editing
### 3.1. Install [`gimp`](https://www.freshports.org/graphics/gimp/)
```
pkg install gimp
```
### 3.2. Install [`inkscape`](https://www.freshports.org/graphics/inkscape/)
```
pkg install inkscape
```
### 3.3. Install [`openscad`](https://www.freshports.org/cad/openscad/)
```
pkg install openscad
```
## 4. Multimedia
### 4.1. Install [`mpv`](https://www.freshports.org/multimedia/mpv/)
```
pkg install mpv
```
