# [Install JAVA & Browser](https://www.freebsd.org/doc/en_US.ISO8859-1/books/handbook/desktop-browsers.html).

1. JAVA as [OpenJDK](http://www.freshports.org/java/openjdk8) in place of the binary Oracle JRE.
```
pkg install openjdk8
```

2. [IcedTea-Web](http://www.freshports.org/java/icedtea-web) project provides a Free Software web browser plugin
running applets written in the Java.
2.1 Installation by building `icetea-web`
```
pkg install icedtea-web
```

2.2 If you encounter problems installing icedtea-web from the port, install from the pre-build binary.
```
pkg install icedtea-web
```

3. Install [firefox](http://www.freshports.org/www/firefox).
```
pkg install firefox
```

4. Install [thunderbird](http://www.freshports.org/mail/thunderbird)
```
pkg install thunderbird
```

5. Install [chrome](http://www.freshports.org/www/chromium)
```
pkg install chromium
```
Note that the [executable is actually chrome and not chromium.](https://www.freebsd.org/doc/en_US.ISO8859-1/books/handbook/desktop-browsers.html)

6. Install [surf](https://www.freshports.org/www/surf)
```
pkg install surf-browser
```

7. Install [`mupdf`](https://www.freshports.org/graphics/mupdf/); a lightweight PDF, XPS, and E-book viewer.
```
cd /usr/ports/graphics/mupdf/ && make install clean
```
