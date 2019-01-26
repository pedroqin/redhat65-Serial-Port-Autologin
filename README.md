# redhat65-Serial-Port-Autologin


> This autologin file is used for RHEL/Centos 6 's Serial port autologin,


1. place it on `/usr/sbin` and run `chmod +x /usr/sbin/autologin`
2. do some change on `/etc/init/serial.conf`:

replace `exec /sbin/agetty /dev/$DEV $SPEED vt100-nav` with `exec /sbin/agetty -n -l /usr/sbin/autologin $SPEED $DEV linux`
```shell
[root@localhost ~]# cat /etc/init/serial.conf |grep -v ^#

start on fedora.serial-console-available DEV=* and stopped rc RUNLEVEL=[2345]
stop on runlevel [S016]

instance $DEV
respawn
pre-start exec /sbin/securetty $DEV
exec /sbin/agetty -n -l /usr/sbin/autologin $SPEED $DEV linux 
post-stop exec /sbin/initctl emit --no-wait fedora.serial-console-available DEV=$DEV SPEED=$SPEED
usage 'DEV=ttySX SPEED=Y  - where X is console id and Y is baud rate'
[root@localhost ~]#
```
