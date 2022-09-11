# 0. Connect to the TryHackMe network using OpenVPN
To hack machines on TryHackMe you need to connect to our network.
You can connect through OpenVPN, or a Kali Linux machine controlled in your browser.

```sh
% ssh -i "sample-key.pem" kali@xxx.xxx.xxx.xxx
┌──(kali㉿kali)-[~]
└─$

┌──(kali㉿kali)-[~]
└─$ whoami
kali

┌──(kali㉿kali)-[~]
└─$ cd thm

┌──(kali㉿kali)-[~/thm]
└─$ sudo openvpn sample.ovpn
...
...
```


# 1. Execute GoBuster
Execute GoBuster command to brute-force website pages.

```sh
┌──(kali㉿kali)-[~/thm]
└─$ ping 10.10.210.124
PING 10.10.210.124 (10.10.210.124) 56(84) bytes of data.
64 bytes from 10.10.210.124: icmp_seq=1 ttl=63 time=200 ms
64 bytes from 10.10.210.124: icmp_seq=2 ttl=63 time=204 ms
^C
--- 10.10.210.124 ping statistics ---
2 packets transmitted, 2 received, 0% packet loss, time 1002ms
rtt min/avg/max/mdev = 199.795/201.980/204.165/2.185 ms

┌──(kali㉿kali)-[~/thm]
└─$ pwd;ls -la
/home/kali/thm
total 88
drwxr-xr-x  2 kali kali  4096 Sep 10 07:29 .
drwxr-xr-x 19 kali kali  4096 Sep 10 07:14 ..
-rw-r--r--  1 kali kali  8315 Sep 10 05:49 fleetprecise64.ovpn
-rw-r--r--  1 root root 62165 Sep 10 06:02 wordlist.txt
-rw-r--r--  1 kali kali    21 Sep 10 07:29 wordlist1.txt

┌──(kali㉿kali)-[~/thm]
└─$ head wordlist1.txt
images
bank-transfer
abacus
abdomen
abdominal
abide
abiding
ability
ablaze
able
┌──(kali㉿kali)-[~/thm]
└─$ gobuster -u http://10.10.210.124 -w wordlist1.txt dir
===============================================================
Gobuster v3.1.0
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)
===============================================================
[+] Url:                     http://10.10.210.124
[+] Method:                  GET
[+] Threads:                 10
[+] Wordlist:                wordlist1.txt
[+] Negative Status codes:   404
[+] User Agent:              gobuster/3.1.0
[+] Timeout:                 10s
===============================================================
2022/09/10 07:31:02 Starting gobuster in directory enumeration mode
===============================================================
/images               (Status: 301) [Size: 179] [--> /images/]
/bank-transfer        (Status: 200) [Size: 4562]

===============================================================
2022/09/10 07:31:03 Finished
===============================================================
┌──(kali㉿kali)-[~/thm]
└─$ 
```
