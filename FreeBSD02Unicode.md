# Setup Unicode

By enabling a locale using UTF-8 character encoding,
the system can understand and display each of the 1112064 characters in the Unicode character set,
instead of just US ASCII as its default with LANG=C.

1. For a list of available UTF-8 locale on your computer.
```
locale -a | grep UTF-8
```
  Note:
  - `en_US.UTF-8` is American
  - The `LANG=xx_YY.ZZZZ` environment variable sets the system locale to
    * language code `xx`,
    * country code `YY`, and
    * character encoding `ZZZZ`

Language and country code affect default application language, number formatting,
date and time formatting, string collation, currency settings, and more.

2. Enable UTF-8 by opening the `vi /etc/login.conf` file.

| Already in file                | Add below in file    |
| ------------------------------ | -------------------- |
| `:pseudoterminals=unlimited:\` | `:charset=UTF-8:\`   |
| `:priority=0:\`                | `:lang=en_US.UTF-8:` |
| `:ignoretime@:\`               |                      |
| `:umask=022:`                  |                      |

3. Changes made in step-2 must be rebuilt in the login database by
```cap_mkdb /etc/login.conf```

4. Specify this new locale for non login uses with `vi /etc/profile` and adding
```
LANG=en_US.UTF-8;       [this will export LANG]
CHARSET=UTF-8;          [this will export CHARSET]
GDM_LANG=en_US.UTF-8;   [this will export GDM_LANG]
```

5. Check changes made in step-4 ([might require `reboot`](https://cooltrainer.org/a-freebsd-desktop-howto/)) with
```
locale
```
  Output should look something like
```
LANG=en_US.UTF-8
LC_CTYPE="en_US.UTF-8"
LC_COLLATE="en_US.UTF-8"
LC_TIME="en_US.UTF-8"
LC_NUMERIC="en_US.UTF-8"
LC_MONETARY="en_US.UTF-8"
LC_MESSAGES="en_US.UTF-8"
LC_ALL=en_US.UTF-8
```
