# [Install JAVA & Browser](https://www.freebsd.org/doc/en_US.ISO8859-1/books/handbook/desktop-browsers.html).

1. JAVA as [OpenJDK](https://www.freshports.org/search.php?query=openjdk&search=go&num=10&stype=name&method=match&deleted=excludedeleted&start=1&casesensitivity=caseinsensitive) in place of the binary Oracle JRE.

[OpenJDK8](http://www.freshports.org/java/openjdk8)
```
pkg install openjdk8
```
[OpenJDK24](http://www.freshports.org/java/openjdk24)
```
pkg install openjdk24
```


2. [IcedTea-Web](http://www.freshports.org/java/icedtea-web) project provides a Free Software web browser plugin
running applets written in the Java.
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

7. Install [lynx](https://www.freshports.org/www/lynx/) if all the above browsers are resource hungry.
```
pkg install lynx
```

## Change default Java version
1. Check for current version used
```
JAVAVM_DRYRUN=yes java
```
2. Check if the desired version is installed (e.g. OpenJDK24)
```
JAVAVM_DRYRUN=yes JAVA_VERSION=24 java
```
Alternatively, get the list of installed Java versions using
```
cat /usr/local/etc/javavms
```
3. Set for specific Java version
```
JAVA_VERSION=24+ javac MyClass.java
```
