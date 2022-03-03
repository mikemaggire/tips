# Linux

## Misc

- [Command-Line Printing and Options](https://www.cups.org/doc/options.html)
- [Install Samba on Raspi](https://www.inpact-hardware.com/article/1013/transformez-votre-raspberry-pi-4-en-nas)

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
# make it executable
sudo chmod +x {path/filename}
# make it readable and writtable
sudo chmod +rw {path/filename}
```
