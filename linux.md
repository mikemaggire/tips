# Linux

## Usefull commands

### dig: (Domain Information Groper) lookup info into DNS servers

```shell
$ dig {url} +nostats +nocomments +nocmd
# options
# +trace
```


[Comment utiliser la commande Dig sous Linux](https://www.hostinger.fr/tutoriels/comment-utiliser-la-commande-dig-sous-linux)

## Printing

- [How to Set Up a Print Server on Raspberry Pi Using CUPS and Ubuntu 20.04](https://medium.com/@liviu.ciulinaru/how-to-set-up-a-print-server-on-raspberry-pi-using-cups-and-ubuntu-20-04-132c83e3c2b0)
- [Command-Line Printing and Options](https://www.cups.org/doc/options.html)

```text
# Config /etc/cups/cupsd.conf
...
# Listen localhost:631
Port 631 # for a LAN access
...
# Restrict access to the server...
<Location />
  Order allow,deny
  Allow @LOCAL # for a LAN access
</Location>

# Restrict access to the admin pages...
<Location /admin>
  Order allow,deny
  Allow @LOCAL # for a LAN access
</Location>
...
```

```bash
sudo service cups {start|stop|restart}
sudo usermod -a -G lpadmin {user}

# admin acces in web mode
https://{local-printer-ip}:631/admin

# command line
# display printers
sudo lpinfo -v 

```

To remove color characters from a file, use the `sed` command

```bash
sed 's/\x1b\[[0-9;]*m//g' {input_file} > {output_file}
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
- [How to force user grouop ownership on a samba share](https://www.thegeekdiary.com/how-to-force-user-group-ownership-of-files-on-a-samba-share/)

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
   map archive = no
...
[usbshare]
   comment = share USB drive on the Raspberry Pi
   path = /data/usbshare
   valid users = lool,me # take care of the writting, valid users with an 's'
   directory mask = 0750 # or 0775
   create mask = 0750 # or 0775
   browseable = yes
   writeable = yes # take care of the writting, writeable with 'ea'
   read only = no
   guest ok = no
   force group = {samba_user_group }
   force create mode = 0750 # or 0775
   force directory mode = 0750 # or 0775
   public = no
...
```

samba log files location is defined within the config file, by default they're here: `/var/log/samba/`

There's an overall log file and a file per client 

Configured samba's users can be seen with pbedit
```
sudo pdbedit -L -v
```

Give the right permissions to the share drive

```bash
mkdir /data/usbshare
chown :sambashare /data/usbshare
chmod 775 /data/usbshare
sudo chmod g+s /data/usbshare
```
### On the client side

On the client side, a drive must be mounted and configured with fstab: `/etc/fstab`, and cifs libs must be installed

```bash
sudo apt install cifs-utils
```

```text
...
//{IPofTheSambaServer}/{SambaServerShareName} {localDriveName} cifs _netdev,rw,uid=1000,auto,vers=3.0,credentials=/root/.sambacredentials,iocharset=utf8,file_mode=0774,dir_mode=0775 0 0
...
```

For automount, we store the credentials in a file that must be in the /root/ path: `/root/.sambacredientials`

```text
username={yourUserNameOnTheSambaServer}
password={yourPasswordOnTheSambaServer}
```
Then manually mount/unmount the drive

```bash
# mount drives configured on this file
sudo mount -a -v
# to unmount a drive
sudo umount {directory}
```
