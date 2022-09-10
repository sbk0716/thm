# How To Setup A Kali Instance In AWS (With RDP)

## 1. Connect to instance

```sh
% ssh -i "sample-key.pem" kali@xxx.xxx.xxx.xxx
┌──(kali㉿kali)-[~]
└─$

┌──(kali㉿kali)-[~]
└─$ whoami
kali

┌──(kali㉿kali)-[~]
└─$ uname -a
Linux kali 5.18.0-kali5-cloud-amd64 #1 SMP PREEMPT_DYNAMIC Debian 5.18.5-1kali6 (2022-07-07) x86_64 GNU/Linux

┌──(kali㉿kali)-[~]
└─$ pwd;ls -la
/home/kali
total 56
drwxr-xr-x 5 kali kali  4096 Sep 10 04:10 .
drwxr-xr-x 3 root root  4096 Sep 10 04:06 ..
-rw-r--r-- 1 kali kali   220 May 12 15:05 .bash_logout
-rw-r--r-- 1 kali kali  5551 Aug 17 05:14 .bashrc
-rw-r--r-- 1 kali kali  3526 May 12 15:05 .bashrc.original
drwxr-xr-x 3 kali kali  4096 Aug 17 05:12 .config
drwxr-xr-x 3 kali kali  4096 Aug 17 05:12 .java
-rw-r--r-- 1 kali kali   807 May 12 15:05 .profile
drwx------ 2 kali kali  4096 Sep 10 04:06 .ssh
-rw------- 1 kali kali    21 Sep 10 04:10 .zsh_history
-rw-r--r-- 1 kali kali 10877 Jul 27 14:52 .zshrc

┌──(kali㉿kali)-[~]
└─$ df -h
Filesystem      Size  Used Avail Use% Mounted on
udev            472M     0  472M   0% /dev
tmpfs            98M  452K   97M   1% /run
/dev/xvda1       12G  8.3G  2.8G  76% /
tmpfs           487M     0  487M   0% /dev/shm
tmpfs           5.0M     0  5.0M   0% /run/lock
/dev/xvda15     124M  270K  124M   1% /boot/efi
tmpfs            98M     0   98M   0% /run/user/0
tmpfs            98M     0   98M   0% /run/user/1000

┌──(kali㉿kali)-[~]
└─$
```

## 2. Set password

```sh
┌──(kali㉿kali)-[~]
└─$ sudo passwd kali
New password:
Retype new password:
passwd: password updated successfully

┌──(kali㉿kali)-[~]
└─$
```

## 3. Install Xfce4 & xrdp

```sh
┌──(kali㉿kali)-[~]
└─$ ls
xfce4.sh

┌──(kali㉿kali)-[~]
└─$ ls -l
total 0
-rw-r--r-- 1 kali kali 0 Sep 10 04:21 xfce4.sh

┌──(kali㉿kali)-[~]
└─$

┌──(kali㉿kali)-[~]
└─$ vim xfce4.sh

┌──(kali㉿kali)-[~]
└─$ cat xfce4.sh
#!/bin/sh
echo "[i] Updating and upgrading Kali (this will take a while)"
apt-get update
apt-get dist-upgrade -y

echo "[i] Installing Xfce4 & xrdp (this will take a while as well)"
apt-get install -y kali-desktop-xfce xorg xrdp

echo "[i] Configuring xrdp to listen to port 3390 (but not starting the service)"
sed -i 's/port=3389/port=3390/g' /etc/xrdp/xrdp.ini

┌──(kali㉿kali)-[~]
└─$ chmod +x xfce4.sh

┌──(kali㉿kali)-[~]
└─$ ls -l
total 4
-rwxr-xr-x 1 kali kali 364 Sep 10 04:22 xfce4.sh

┌──(kali㉿kali)-[~]
└─$

┌──(kali㉿kali)-[~]
└─$ sudo ./xfce4.sh
...
...
Processing triggers for libc-bin (2.34-4) ...
Processing triggers for dbus (1.14.0-2) ...
[i] Configuring xrdp to listen to port 3390 (but not starting the service)

┌──(kali㉿kali)-[~]
└─$ sudo systemctl enable xrdp --now
Synchronizing state of xrdp.service with SysV service script with /lib/systemd/systemd-sysv-install.
Executing: /lib/systemd/systemd-sysv-install enable xrdp
Created symlink /etc/systemd/system/multi-user.target.wants/xrdp.service → /lib/systemd/system/xrdp.service.

┌──(kali㉿kali)-[~]
└─$
┌──(kali㉿kali)-[~]
└─$ sudo systemctl status xrdp
● xrdp.service - xrdp daemon
     Loaded: loaded (/lib/systemd/system/xrdp.service; enabled; preset: disable>
     Active: active (running) since Sat 2022-09-10 04:37:25 UTC; 27s ago
       Docs: man:xrdp(8)
             man:xrdp.ini(5)
    Process: 31826 ExecStartPre=/bin/sh /usr/share/xrdp/socksetup (code=exited,>
    Process: 31834 ExecStart=/usr/sbin/xrdp $XRDP_OPTIONS (code=exited, status=>
   Main PID: 31835 (xrdp)
      Tasks: 1 (limit: 1131)
     Memory: 1.4M
        CPU: 12ms
     CGroup: /system.slice/xrdp.service
             └─31835 /usr/sbin/xrdp

Sep 10 04:37:24 kali systemd[1]: Starting xrdp daemon...
Sep 10 04:37:24 kali xrdp[31834]: [INFO ] address [0.0.0.0] port [3390] mode 1
Sep 10 04:37:24 kali xrdp[31834]: [INFO ] listening to port 3390 on 0.0.0.0
...
...
┌──(kali㉿kali)-[~]
└─$ sudo reboot
%
% ssh -i "sample-key.pem" kali@xxx.xxx.xxx.xxx
┌──(kali㉿kali)-[~]
└─$ sudo vim /etc/polkit-1/localauthority/50-local.d/45-allow-colord.pkla

┌──(kali㉿kali)-[~]
└─$ sudo cat /etc/polkit-1/localauthority/50-local.d/45-allow-colord.pkla
[Allow Colord all Users]
Identity=unix-user:*
Action=org.freedesktop.color-manager.create-device;org.freedesktop.color-manager.create-profile;org.freedesktop.color-manager.delete-device;org.freedesktop.color-manager.delete-profile;org.freedesktop.color-manager.modify-device;org.freedesktop.color-manager.modify-profile
ResultAny=no
ResultInactive=no
ResultActive=yes

┌──(kali㉿kali)-[~]
└─$
```


## 4. Install OpenVPN

```sh
┌──(kali㉿kali)-[~/thm]
└─$ sudo apt install openvpn
Reading package lists... Done
Building dependency tree... Done
Reading state information... Done
openvpn is already the newest version (2.6.0~really2.5.7-0kali1).
openvpn set to manually installed.
The following packages were automatically installed and are no longer required:
  libfmt8 libhttp-server-simple-perl libpoppler118 python3-dataclasses-json
  python3-limiter python3-marshmallow python3-marshmallow-enum
  python3-mypy-extensions python3-ntp python3-responses python3-spyse
  python3-token-bucket python3-typing-inspect sphinx-rtd-theme-common
Use 'sudo apt autoremove' to remove them.
0 upgraded, 0 newly installed, 0 to remove and 0 not upgraded.

┌──(kali㉿kali)-[~/thm]
└─$
┌──(kali㉿kali)-[~/thm]
└─$ which openvpn
/usr/sbin/openvpn

┌──(kali㉿kali)-[~/thm]
└─$ openvpn --version
OpenVPN 2.5.7 x86_64-pc-linux-gnu [SSL (OpenSSL)] [LZO] [LZ4] [EPOLL] [PKCS11] [MH/PKTINFO] [AEAD] built on Jul  5 2022
library versions: OpenSSL 3.0.5 5 Jul 2022, LZO 2.10
Originally developed by James Yonan
Copyright (C) 2002-2022 OpenVPN Inc <sales@openvpn.net>
Compile time defines: enable_async_push=no enable_comp_stub=no enable_crypto_ofb_cfb=yes enable_debug=yes enable_def_auth=yes enable_dependency_tracking=no enable_dlopen=unknown enable_dlopen_self=unknown enable_dlopen_self_static=unknown enable_fast_install=needless enable_fragment=yes enable_iproute2=no enable_libtool_lock=yes enable_lz4=yes enable_lzo=yes enable_maintainer_mode=no enable_management=yes enable_multihome=yes enable_option_checking=no enable_pam_dlopen=no enable_pedantic=no enable_pf=yes enable_pkcs11=yes enable_plugin_auth_pam=yes enable_plugin_down_root=yes enable_plugins=yes enable_port_share=yes enable_selinux=no enable_shared=yes enable_shared_with_static_runtimes=no enable_silent_rules=no enable_small=no enable_static=yes enable_strict=no enable_strict_options=no enable_systemd=yes enable_werror=no enable_win32_dll=yes enable_x509_alt_username=yes with_aix_soname=aix with_crypto_library=openssl with_gnu_ld=yes with_mem_check=no with_openssl_engine=auto with_sysroot=no

┌──(kali㉿kali)-[~/thm]
└─$ sudo openvpn sample.ovpn
...
...
┌──(kali㉿kali)-[~/thm]
└─$ 
```
