
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

# 1. Does the target (10.10.40.122)respond to ICMP (ping) requests (Y/N)?
```sh
root@ip-10-10-248-187:~# sudo nmap -PE 10.10.40.122

Starting Nmap 7.60 ( https://nmap.org ) at 2022-09-16 22:31 BST
Nmap scan report for ip-10-10-40-122.eu-west-1.compute.internal (10.10.40.122)
Host is up (0.00094s latency).
Not shown: 995 filtered ports
PORT     STATE SERVICE
21/tcp   open  ftp
53/tcp   open  domain
80/tcp   open  http
135/tcp  open  msrpc
3389/tcp open  ms-wbt-server
MAC Address: 02:C7:10:49:5F:69 (Unknown)

Nmap done: 1 IP address (1 host up) scanned in 16.94 seconds
root@ip-10-10-248-187:~# 
root@ip-10-10-248-187:~# ping 10.10.40.122
PING 10.10.40.122 (10.10.40.122) 56(84) bytes of data.
^C
--- 10.10.40.122 ping statistics ---
37 packets transmitted, 0 received, 100% packet loss, time 36861ms

root@ip-10-10-248-187:~# 
```

# 2. Perform an Xmas scan on the first 999 ports of the target -- how many ports are shown to be open or filtered?
```sh
root@ip-10-10-248-187:~# sudo nmap -p 1-999 -sX 10.10.40.122 -Pn -vv

Starting Nmap 7.60 ( https://nmap.org ) at 2022-09-16 22:39 BST
Initiating ARP Ping Scan at 22:39
Scanning 10.10.40.122 [1 port]
Completed ARP Ping Scan at 22:39, 0.23s elapsed (1 total hosts)
Initiating Parallel DNS resolution of 1 host. at 22:39
Completed Parallel DNS resolution of 1 host. at 22:39, 0.00s elapsed
Initiating XMAS Scan at 22:39
Scanning ip-10-10-40-122.eu-west-1.compute.internal (10.10.40.122) [999 ports]
Completed XMAS Scan at 22:39, 21.08s elapsed (999 total ports)
Nmap scan report for ip-10-10-40-122.eu-west-1.compute.internal (10.10.40.122)
Host is up, received arp-response (0.00011s latency).
All 999 scanned ports on ip-10-10-40-122.eu-west-1.compute.internal (10.10.40.122) are open|filtered because of 999 no-responses
MAC Address: 02:C7:10:49:5F:69 (Unknown)

Read data files from: /usr/bin/../share/nmap
Nmap done: 1 IP address (1 host up) scanned in 21.43 seconds
           Raw packets sent: 1999 (79.948KB) | Rcvd: 1 (28B)
root@ip-10-10-248-187:~# 
```

# 3. Perform a TCP SYN scan on the first 5000 ports of the target -- how many ports are shown to be open?
```sh
root@ip-10-10-248-187:~# sudo nmap -p 1-5000 -sS 10.10.40.122 -Pn -vv

Starting Nmap 7.60 ( https://nmap.org ) at 2022-09-16 22:43 BST
Initiating ARP Ping Scan at 22:43
Scanning 10.10.40.122 [1 port]
Completed ARP Ping Scan at 22:43, 0.22s elapsed (1 total hosts)
Initiating Parallel DNS resolution of 1 host. at 22:43
Completed Parallel DNS resolution of 1 host. at 22:43, 0.00s elapsed
Initiating SYN Stealth Scan at 22:43
Scanning ip-10-10-40-122.eu-west-1.compute.internal (10.10.40.122) [5000 ports]
Discovered open port 3389/tcp on 10.10.40.122
Discovered open port 80/tcp on 10.10.40.122
Discovered open port 53/tcp on 10.10.40.122
Discovered open port 21/tcp on 10.10.40.122
Discovered open port 135/tcp on 10.10.40.122
Increasing send delay for 10.10.40.122 from 0 to 5 due to 11 out of 32 dropped probes since last increase.
SYN Stealth Scan Timing: About 35.77% done; ETC: 22:44 (0:00:56 remaining)
Increasing send delay for 10.10.40.122 from 5 to 10 due to 11 out of 28 dropped probes since last increase.
Increasing send delay for 10.10.40.122 from 10 to 20 due to 11 out of 34 dropped probes since last increase.
SYN Stealth Scan Timing: About 62.73% done; ETC: 22:45 (0:00:52 remaining)
Increasing send delay for 10.10.40.122 from 20 to 40 due to 11 out of 32 dropped probes since last increase.
Increasing send delay for 10.10.40.122 from 40 to 80 due to 11 out of 32 dropped probes since last increase.
SYN Stealth Scan Timing: About 70.54% done; ETC: 22:46 (0:00:59 remaining)
SYN Stealth Scan Timing: About 75.21% done; ETC: 22:48 (0:01:07 remaining)
SYN Stealth Scan Timing: About 80.56% done; ETC: 22:49 (0:01:05 remaining)

root@ip-10-10-248-187:~# 
```

# 4. Deploy the ftp-anon script against the box. Can Nmap login successfully to the FTP server on port 21? (Y/N)
```sh
root@ip-10-10-248-187:~# sudo nmap --script=ftp-anon -p 21 10.10.40.122 -vv

Starting Nmap 7.60 ( https://nmap.org ) at 2022-09-16 22:51 BST
NSE: Loaded 1 scripts for scanning.
NSE: Script Pre-scanning.
NSE: Starting runlevel 1 (of 1) scan.
Initiating NSE at 22:51
Completed NSE at 22:51, 0.00s elapsed
Initiating ARP Ping Scan at 22:51
Scanning 10.10.40.122 [1 port]
Completed ARP Ping Scan at 22:51, 0.22s elapsed (1 total hosts)
Initiating Parallel DNS resolution of 1 host. at 22:51
Completed Parallel DNS resolution of 1 host. at 22:51, 0.00s elapsed
Initiating SYN Stealth Scan at 22:51
Scanning ip-10-10-40-122.eu-west-1.compute.internal (10.10.40.122) [1 port]
Discovered open port 21/tcp on 10.10.40.122
Completed SYN Stealth Scan at 22:51, 0.22s elapsed (1 total ports)
NSE: Script scanning 10.10.40.122.
NSE: Starting runlevel 1 (of 1) scan.
Initiating NSE at 22:51
Completed NSE at 22:51, 30.01s elapsed
Nmap scan report for ip-10-10-40-122.eu-west-1.compute.internal (10.10.40.122)
Host is up, received arp-response (0.00022s latency).
Scanned at 2022-09-16 22:51:25 BST for 31s

PORT   STATE SERVICE REASON
21/tcp open  ftp     syn-ack ttl 128
| ftp-anon: Anonymous FTP login allowed (FTP code 230)
|_Can't get directory listing: TIMEOUT
MAC Address: 02:C7:10:49:5F:69 (Unknown)

NSE: Script Post-scanning.
NSE: Starting runlevel 1 (of 1) scan.
Initiating NSE at 22:51
Completed NSE at 22:51, 0.00s elapsed
Read data files from: /usr/bin/../share/nmap
Nmap done: 1 IP address (1 host up) scanned in 30.97 seconds
           Raw packets sent: 3 (116B) | Rcvd: 3 (116B)
root@ip-10-10-248-187:~# 
```

FYI:
https://hana-shin.hatenablog.com/entry/2022/01/04/200357