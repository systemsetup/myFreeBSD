# Shortcuts for [dwm](https://dwm.suckless.org/tutorial/)

| Purpose                           | Key combinations         |
| --------------------------------- | ------------------------ |
| launch terminal                   | `[Shift]+[Alt]+[Enter]`  |
| focus on another tag              | `[Alt]+[tag number]`     |
| move to another terminal          | `[Alt]+[j] or [Alt]+[k]` |
| kill a terminal (window)          | `[Shift]+[Alt]+[c]`      |
| float mode                        | `[Alt]+[f]`              |
| tiling mode (fullscreen terminal) | `[Alt]+[t]`              |
| horizontal tiles in tiling mode   | `[Alt]+[i]`              |
| vertical tiles in tiling mode     | `[Alt]+[d]`              |
| monocale mode (fullscreen window) | `[Alt]+[m]`              |
| quit dwm                          | `[Shift]+[Alt]+[q]`      |

# Commands for system information

| Purpose                       | Command                |
| ----------------------------- | ---------------------- |
| kernel name                   | `uname`                |
| host**n**ame                  | `uname -n`             |
| kernel **v**ersion            | `uname -v`             |
| kernel **r**elease            | `uname -r`             |
| **m**achine hardware          | `uname -m`             |
| **p**rocessor architecture    | `uname -p`             |
| **a**ll the above info        | `uname -a`             |
| cpu information               | `dmesg \| grep CPU`    |
| real and available memory     | `dmesg \| grep memory` |
| connected devices information | `geom disk list`       |
| see availabe hard drive space | `df -h`                |
| battery life/status           | `apm`                  |
| how long a system has been up | `uptime`               |
| kill `startx` to enter console| [Alt]+[Shift]+[F1]     |
| list global environment       | `setenv`               |
| set global environment        | `setenv Dummy "this"`  |
| unset a variable              | `unsetenv Dummy`       |
