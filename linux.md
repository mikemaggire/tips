# Linux

## Misc

## Printing

- [How to Set Up a Print Server on Raspberry Pi Using CUPS and Ubuntu 20.04](https://medium.com/@liviu.ciulinaru/how-to-set-up-a-print-server-on-raspberry-pi-using-cups-and-ubuntu-20-04-132c83e3c2b0)
- [Command-Line Printing and Options](https://www.cups.org/doc/options.html)

```text
# Config /etc/cups/cupsd.conf
...
# Listen localhost:631
Port 631
...
# Restrict access to the server...
<Location />
  Order allow,deny
  Allow @LOCAL
</Location>

# Restrict access to the admin pages...
<Location /admin>
  Order allow,deny
  Allow @LOCAL
</Location>
...
```

```bash
sudo service cups {start|stop|restart}
sudo usermod -a -G lpadmin {user}
https://{local-printer-ip}:631/admin
```

## Cron

```bash
# restart cron deamon
sudo /etc/init.d/cron start
# list cron tasks
crontab -l
# edit cron tasks
crontab -e
# watch logs
grep CRON /var/log/syslog
```

## Permissions

```bash
# display the current `local` permission/ownership
ls -l {directory|filename}

# make it executable
sudo chmod +x {directory|filename}

# make it readeable and writeable
sudo chmod +rw {directory|filename}

# changing the owner a,d/or the group of a directory/file
sudo chown -R [{user}]:[{group}] {directory|filename}
```

[How do I find out owner / group name for a file?](https://www.cyberciti.biz/faq/unix-linux-find-file-owner-name/)

Nota: if {directory|filename} is on remote shared drive (eg. a CIFS samba), then you need to check the samba server permissions in addition

```bash
# on the samba server (V4.13.17-Ubuntu), display connected users and SharePaths
sudo smbstatus -v
# then check permissions on the SharePaths according to the connected user
ls -la {SharePath}
```

## CIFS / samba 

> wording: `samba` software implements `CIFS` network protocol. [>> more](https://unix.stackexchange.com/questions/34742/cifs-vs-samba-what-are-the-differences)

- [Install Samba on Raspi](https://www.inpact-hardware.com/article/1013/transformez-votre-raspberry-pi-4-en-nas)
- [How to Setup a Raspberry Pi Samba Server](https://pimylifeup.com/raspberry-pi-samba/) 2022-02-14

```bash
# samba server (V4.13.17-Ubuntu)
# display samba statut on the server side
sudo smbstatus -v
# start / stop samba server
sudo service smbd {start|stop|restart}
```

samba configuration file is `/etc/samba/smb.conf`

```
...
[global]
   client min protocol = SMB2
   client max protocol = SMB3
...
[usbshare]
   comment = share USB drive on the Raspberry Pi
   path = /data/usbshare
   valid users = lool,me # take care of the writting, valid users with an 's'
   directory mask = 0750
   create mask = 0750
   browseable = yes
   writeable = yes # take care of the writting, writeable with 'ea'
   read only = no
   guest ok = no
...
```

samba log files location is defined within the config file, by default they're here: `/var/log/samba/`

There's an overall log file and a file per client 

Configured samba's users can be seen with pbedit
```
sudo pdbedit -L -v
```

On the client side, a drive must be mounted

```
# edit mounted drive configuration
sudo nano /etc/fstab
# mount drives configured on this file
sudo mount -a
# to unmount a drive
sudo umount {directory}

```
