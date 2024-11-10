## [Upgrade/Update FreeBSD](https://docs.freebsd.org/en/books/handbook/cutting-edge/)

### Update FreeBSD
Even if you are upgrading to a different FreeBSD release you must first update the current FreeBSD, i.e, apply all the current patches.

```
freebsd-update fetch
freebsd-update install
```
then, reboot to enter the updated system.

1. Do you need to reboot
   ```
   freebsd-version -k && uname -r
   ```
   If same outputs you don't need to reboot.
2. Automatically check (and download) for patches daily by entering the below statement in `vi /etc/crontab`
   ```
   @daily                                  root    freebsd-update cron
   ```
   To install the downloaded updates manually run the command
   ```
   freebsd-update install
   ```
4. Rollback the current update
   ```
   freebsd-update rollback
   ```

### Upgrade FreeBSD
1. Run the command
   ```
   freebsd-update -r X.Y-RELEASE upgrade
   ```
2. Apply/install the downloads
  ```
  freebsd-update install
  ```
3. Upgrade installed packages
   ```
   pkg-static upgrade -f
   ```
   * upgrade installed ports
     ```
     portmaster -af
     ```
4. Tie up loose ends
  ```
  freebsd-update install
  ```

### Update documentation
1. Download the source
   ```
   git clone https://git.FreeBSD.org/doc.git /usr/doc
   ```
2. Update
   ```
   cd /usr/doc
   make
   ```
3. Fetch future source update
   ```
   cd /usr/doc
   git pull
   ```
