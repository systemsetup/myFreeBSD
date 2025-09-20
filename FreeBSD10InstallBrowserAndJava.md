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
Based on the [commands](https://man.freebsd.org/cgi/man.cgi?query=javavm&manpath=FreeBSD+12.1-RELEASE+and+Ports#end)
1. Check for current version used
```
JAVAVM_DRYRUN=yes java
```
Alternatively,
```
java -version
```
2. Check if the desired version is installed (e.g. OpenJDK24)
```
JAVAVM_DRYRUN=yes JAVA_VERSION=24 java
```
Alternatively, get the list of installed Java versions using
```
cat /usr/local/etc/javavms
```
3. Compile a `MyClass.java` using a specific Java version (e.g. &ge; OpenJDK24)
```
JAVA_VERSION=24+ javac MyClass.java
```
4. Set a specific Java version
For JDK or JRE the operating system environment variable is `JAVA_HOME`.
That is, Java application use this variable to find where the runtime (JVM) is installed.

4.1. set the value of `JAVA_HOME` to desired Java version
In FreeBSD, you set `JAVA_HOME` environment in
* `/etc/csh.cshrc` for all users
* `/usr/home/<user>/.cshrc` for a specific user

such that the variable value is determined from running `cat /usr/local/etc/javavms`.

For example, for setting OpenJDK24 as the default Java version, do `vi /etc/csh.cshrc` and enter
```
# System-wide .cshrc file for csh(1).
setenv JAVA_HOME /usr/local/openjdk24
setenv PATH $JAVA_HOME/bin:$PATH
```

4.2. Make the path `/etc/csh.cshrc` available to source
```
source /etc/csh.cshrc
```

4.3. Switch to another Java version (e.g. OpenJDK17)
```
# System-wide .cshrc file for csh(1).
setenv JAVA_HOME /usr/local/openjdk17
setenv PATH $JAVA_HOME/bin:$PATH
```
Then, repeat 4.2. to make the new path available to source.

4.4. Check the Java version
```
java -version
```
