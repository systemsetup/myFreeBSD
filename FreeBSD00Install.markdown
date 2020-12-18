# Install FreeBSD

[Download FreeBSD .img](https://www.freebsd.org/where.html)

[Win32DiskImager](https://sourceforge.net/projects/win32diskimager/)

Plugin the usb containing FreeBSD and boot from it.

Now follow:

1. Boot MultiUser
2. Default Keyboard Keymap
3. Except the (3) debuggers choose all options (spacebar for selection; doc, src, txt, lib32, ports, etc...)
4. Choose `zfs`
5. For zfs configuration, choose default except the following:
   * Rename pool name to hostname (eg. neuralgraphszpool if you chose it as the hostname; notice hostname=neuralgraphs or pool name is neuralgraphs.comzpool if hostname=neuralgraphs.com)
   * Encrypt Disks? NO
   * Encrypt Swap? NO
   * Swap Size
     if RAM is single digit, Swap Size = 2x physical memory
     if RAM is double digit, Swap Size = 1/2 x physical memory (or if you want you can still choose 2x)
6. Next ZFS Configuration
   Read the texts/description for the options.
   If you have only one hard drive, only option you can select is stripe-no redundancy.
7. Choose the hard drive/s (spacebar)
8. Type and re-type the paraphrase (i.e, a long password).
9. Commit and select through the choices and the installation will proceed.
10. Type and re-type the root password.
11. If there is a wifi card choose the network interface. Note that if you see more than one option (options in addition to em0 Intel(R) PRO/1000 Legacy Network Connection) choose the other option (because it is most likely the actual device driver).
12. Choose and let IPv4 and DHCP do the configuration.
13. The network will scan. If you have wifi service access point (AP) and you know the password select that SSID and enter your pre-shared key (PSK) password.
14. Skip the IPv6 configuration.
15. In "Search" for Resolver Configuration enter hostname (eg. neuralgraphs).
16. Region and Time Zone setup.
17. In System Configuration, choose sshd, moused and ~ntpd~. (don’t choose crash dump and cpu sync)
18. If you see another configuration (step after 17), just choose delete tmp directory option. Although it is encouraged to choose crash dumps (for debugging) I avoid it.
18. If you see another configuration (step after 17), just choose delete tmp directory option. Although it is encouraged to choose crash dumps (for debugging) I avoid it.
19. Now choose add user (one is enough if it is a personal computer).
    ```
    Username: jru
    Full name: J. Random User
    Uid (Leave empty for default):
    Login group [jru]:
    Login group is jru. Invite jru into other groups? []: wheel
    Login class [default]:
    Shell (sh csh tcsh zsh nologin) [sh]: zsh
    Home directory [/home/jru]:
    Home directory permissions (Leave empty for default):
    Use password-based authentication? [yes]:
    Use an empty password? (yes/no) [no]:
    Use a random password? (yes/no) [no]:
    Enter password:
    Enter password again:
    Lock out the account after creation? [no]:
    Username   : jru
    Password   : ****
    Full Name  : J. Random User
    Uid        : 1001
    Class      :
    Groups     : jru wheel
    Home       : /home/jru
    Shell      : /usr/local/bin/zsh
    Locked     : no
    OK? (yes/no): yes
    adduser: INFO: Successfully added (jru) to the user database.
    Add another user? (yes/no): no
    Goodbye!
    ```
    Remember to type `wheel` in Login group. wheel membership is necessary to gain root privileges for administration tasks. You can choose csh if zsh is not available or even sh as default.
    [More on user group setup](https://www.freebsd.org/doc/handbook/users-synopsis.html)
20. At Final Configuration, exit by `Apply configuration and exit installer`.
21. No to manual configuration.
22. Reboot.
23. At first reboot screen, below are some commands for extra information
  - `dmesg` to display or see system specs (hardware) [watch UCBT_BSD10x_006_Post-Installation_Checks.mp4]
  - `reboot` to restart the computer
  - `poweroff` to shutdown the computer
