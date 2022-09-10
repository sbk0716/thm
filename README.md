# kali-on-mac


##
```sh
admin@gw-mac thm % docker run -it kalilinux/kali-rolling
┌──(root㉿9939dd18f35a)-[/]
└─# uname -a
Linux 9939dd18f35a 5.10.47-linuxkit #1 SMP PREEMPT Sat Jul 3 21:50:16 UTC 2021 aarch64 GNU/Linux

┌──(root㉿9939dd18f35a)-[/]
└─# pwd
/

┌──(root㉿9939dd18f35a)-[/]
└─# ls
bin   dev  home  media  opt   root  sbin  sys  usr
boot  etc  lib   mnt    proc  run   srv   tmp  var

┌──(root㉿9939dd18f35a)-[/]
└─# 
┌──(root㉿9939dd18f35a)-[/thm]
└─# ls /dev/
console  fd/      mqueue/  ptmx     random   stderr   stdout   urandom  
core     full     null     pts/     shm/     stdin    tty      zero     
┌──(root㉿9939dd18f35a)-[/thm]
└─# mkdir -p /dev/net

┌──(root㉿9939dd18f35a)-[/thm]
└─# ls /dev/net

┌──(root㉿9939dd18f35a)-[/thm]
└─# ls -la/dev/net
ls: invalid option -- '/'
Try 'ls --help' for more information.

┌──(root㉿9939dd18f35a)-[/thm]
└─# ls -la /dev/net
total 0
drwxr-xr-x 2 root root  40 Sep  3 22:30 .
drwxr-xr-x 6 root root 380 Sep  3 22:30 ..

┌──(root㉿9939dd18f35a)-[/thm]
└─# mknod /dev/net/tun c 10 200

┌──(root㉿9939dd18f35a)-[/thm]
└─# ls -la /dev/net
total 0
drwxr-xr-x 2 root root      60 Sep  3 22:31 .
drwxr-xr-x 6 root root     380 Sep  3 22:30 ..
crw-r--r-- 1 root root 10, 200 Sep  3 22:31 tun

┌──(root㉿9939dd18f35a)-[/thm]
└─# chmod 600 /dev/net/tun

┌──(root㉿9939dd18f35a)-[/thm]
└─# ls -la /dev/net
total 0
drwxr-xr-x 2 root root      60 Sep  3 22:31 .
drwxr-xr-x 6 root root     380 Sep  3 22:30 ..
crw------- 1 root root 10, 200 Sep  3 22:31 tun

┌──(root㉿9939dd18f35a)-[/thm]
└─# 
┌──(root㉿9939dd18f35a)-[/thm]
└─# /etc/init.d/openvpn restart
Stopping virtual private network daemon:.
Starting virtual private network daemon:.

┌──(root㉿9939dd18f35a)-[/thm]
└─# ls -la /dev/net
```
## docker-compose build
```sh
docker-compose exec kali /bin/bash

```

## docker-compose build
```sh
docker-compose exec kali /bin/bash

```# thm
